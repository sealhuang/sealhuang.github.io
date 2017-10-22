---
layout: post
title: Python科学计算中的技巧
category: technique
description: 记录Python科学计算中的一些“坑”
published: true
---

## Intro
近年来，Python在科学计算中的应用越发广泛，基于NumPy和SciPy等基础的数值运算软件包提供的矩阵对象（ndarray）和运算方法，用户可以方便地进行数值分析和处理。尽管这些软件包已经利用底层的C代码对矩阵对象存储和计算等进行了优化，但在实际使用中，如何写出比较高效的代码，让运算最有效率，仍是需要一些试错总结的。该文旨在对这些应用中的trick进行总结，希望对大家有所帮助。

## 多线程处理
伴随着计算机硬件水平的提高，多核CPU以及多CPU已经成为标配，如何充分利用这些硬件资源，提高数据计算的效率，成为大家非常关心的问题。实际上，当前已经有一些工具可以帮助我们实现这一目的，如NumPy可以通过Intel Math Kernel Library (MKL)与高度优化的线性代数库相连（比如BLAS和LAPACK），使NumPy中一些特定的矩阵计算实现多线程，充分利用多核处理器的优势。然而，并不是在所有的实际应用中，都可以有效实现这种多线程处理的，以下将记录一些可能的情况。

首先说明下以下测试的环境：MacBook Pro 13-inch Mid 2012；Intel i5 CPU, 2 cores；8G RAM；Anaconda python 2.7 env, numpy and MKL 2017.0.1。

### 数据类型对调用多线程运算的影响
利用NumPy进行矩阵乘法计算，往往可以通过多线程实现运算的加速，例如，在ipython中输入

    import numpy as np
    x = np.random.rand(1000, 1000)
    y = np.random.rand(1000, 500)
    %timeit x.dot(y)

运行以上代码，我们会发现Python进行会占用100+%的CPU负载（可以用top命令看到），即程序自动调用的多线程进行计算，运行结果是：10 loops, best of 3: 26.1 ms per loop

如果我们对上面的代码稍微进行一些修改，具体如下

    import numpy as np
    x = np.random.rand(1000, 1000)
    x = x.astype(np.float16)
    y = np.random.rand(1000, 500)
    y = np.astype(np.float16)
    %timeit x.dot(y)

运行后，我们发现Python的进程所占用的CPU资源始终没有超过100%，同时，相对上一段脚本耗费了约175倍的时间：1 loops, best of 3: 4.55 s per loop

同样的测试，我们也在float32的数据类型下进行了测试，Python会调用多线程来进行乘法运算，而且相对float64类型，耗费时间更短：100 loops, best of 3: 15.4 ms per loop

通过以上测试可见，*数据类型的不同，是会影响是否调用多线程运算的*。

*在进行小数据分析时，也许这不会成为一个问题，我们可以直接在float64或者float32的数据类型下进行运算，然而当我们要对两个较大的矩阵进行相乘时，而机器的内存又不十分充足，我们则会面临这样一个两难问题：如果不减少数据存储的位数，则由于内存资源的限制，Python不会调用多线程进行乘法运算，如果改变数据类型（如float16），则仍然不会调用多线程。*

面对这类问题，一个可能的解决方案是，仍用float64或float32保存两个矩阵，但每次只对一部分数据进行乘法运算，并将结果保存在一个memmap对象中，如

    import numpy as np
    x = np.random.rand(30000, 30000)
    y = np.random.rand(30000, 20000)
    res = np.memmap('results.npy', dtype='float64', mode='w+', shape=(30000, 20000))
    for i in range(20):
        y_part = y[:, i*1000:(i+1)*1000]
        res[:, i*1000:(i+1)*1000] = x.dot(y_part)

这样，则可以对数据进行分块运算，每次乘法的计算量较小，留出内存资源进行多线程运算。当然，每个数据块的大小要根据实际情况而定，仍需要一些试错。另外，要提高for循环的效率，numba的jit说不定也能有些帮助。

*本文不定期更新，也欢迎各位朋友拍砖赐教。*


