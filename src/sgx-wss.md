# 为什么SGX需要wss指标
多个sgx程序运行的话，内存交换必不可少。
那给每个enclave分配多少内存（在xml里）？
只要估计好wss,就算有一定的EPC swap,也没有问题。

# 如何计算sgx的wss
## 内核模块角度
从内核模块的角度，其实和mlock差不多，不过实现比mlock难，因为定时的清0和统计并不简单

## 从 `/proc/pid/map` 角度
SGX中的内存页地址应该也被 `/proc/pid/map` 所记录，所以说不定也会有访问记录。 
那么就可以用gregg 的 wss 的一样的方法获得数据了。 

