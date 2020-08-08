---
layout: post
title: 使用Python进程池消费流数据
category: technique
description: 如何利用Python的multiprocessing库构建任务进程池，来消费流数据
published: true
---

在数据处理工作中，经常会遇到这样一个场景：

1. 从一个数据源接收流数据（比如利用`Kafka`等框架建立的数据流管道，或是从一个大文件里分块读取到的数据）；

2. 对取得的数据进行处理；

3. 将处理结果发送到另一个地方。

如果数据处理的计算量很小，并且你也不太在意处理的速度，当然可以对接收到的数据顺序地逐个处理。但当你想要尽快完成这些计算，你也许就该考虑使用多进程了（`multiprocessing`）。这样你的程序可以同时处理多个数据，或是在处理一组数据的同时，还可以下载新的数据，并将处理好的结果发出去。

利用Python自带的`multiprocessing`库，可以很容易地实现这个过程。使用已经包装好的进程池（`Pool`）和队列（`Queue`）类，让你不必太在意底层的实现细节。

下面是一个简单的实现实例。

    import multiprocessing as mp  
    import time

    def worker(msg, out_queue):
        print('Processing %s (MP: %s) ' % (msg, mp.current_process().name))
        time.sleep(0.01)
        out_queue.put('Job finished - %s'%(msg))

    def sender(out_queue):
        while True:
            if not out_queue.empty():
                msg = out_queue.get()
                print('Kafka sending %s' % msg)
            else:
                time.sleep(0.01)

    def writer(q):
        # Write to the in-queue
        count = 30
        for ii in range(0, count):
            print('Writing %s' % (ii))
            # Write 'count' numbers into the queue
            q.put(ii)
            time.sleep(0.001)

    if __name__=='__main__':  
        # initialize queues
        data_manager = mp.Manager()
        # this is where we are going to store input data
        in_queue = data_manager.Queue(10)
        # this where we are gonna push them out
        out_queue = data_manager.Queue(10)

        # initialize a message writer
        # the writer would generate data for data stream
        mp.Process(target=writer, args=(in_queue,)).start()

        # initialize a message sender
        # the sender would get the processed results out
        mp.Process(target=sender, args=(out_queue,)).start()

        # create processes pool to consume the data
        # the process in the pool would take input data and
        # out-queue as arguments
        # the data will get out through the out-queue
        pool = mp.Pool(3)

        # daemonize it
        while True:
            if not in_queue.empty():
                # consume the input data
                msg = in_queue.get()
                pool.apply_async(worker, (msg, out_queue))
            else:
                time.sleep(0.01)


在上面这个例子里，我们利用`writer`向数据管道（`in_queue`）中写入数据，模拟接收数据的过程，收到的数据会被送到进程池，分配一个进程进行处理，再将结果写入到出队列（`out_queue`）里，`sender`会不断检测出队列是否有数据，如果有数据就将其发送出去。

这里唯一需要注意的地方是，在使用`Pool`时，如果要使用与其搭配的队列，需要从`multiprocessing.Manager`中进行实例化，它是一个资源管理器，具体介绍可以查阅官方手册。

