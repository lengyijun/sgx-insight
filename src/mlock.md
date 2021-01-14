# 如何给SGX添加mlock功能

在linux中，mlock是一个系统调用，可以`man mlock` 查看。 简单的说，就是防止一段内存所在的页，被交换到交换区中。

在sgx中，mlock的含义是防止一个页从EPC被交换到DRAM中。在intel的sgx中，并没有这个功能，而在有些场景下，我们还是需要这个功能的。所以我们需要修改相关代码，以支持mlock功能。

首先要锁住一个EPC页，就意味着在寻找要被交换出去的页的时候，要忽略这些页。每个页都有一些flag.我们在flag中添加一个MLOCK属性，搜索的时候忽略MLOCK的页，这个页就不会被交换出去了。

然后程序如何和 `/dev/isgx` 通信，告诉 `/dev/isgx` 哪个页要添加标记呢？我们可以在linux-sgx的仓库中搜索 ioctl,发现在psw中有许多ioctl的代码，以葫芦画瓢就行了

由于要ioctl,而ioctl是无法在sgx内执行的，本质上这是一个ocall,不是非常安全。

这种该法，使得开发者要调用mlock非常方便，只需要在edl中加一个 import就行。

注意，这种写法只会影响交换，不会影响释放。在Enclave退出时， 所有页都一定会释放的

我主要参考的是 sgx_oc_cpuidex 这个函数的出现位置