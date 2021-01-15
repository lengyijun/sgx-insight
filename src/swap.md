# Swap

我们在这里讨论EPC页如何交换到DRAM中，

## Q1： 选择哪个EPC页交换到DRAM中？

首先要选择是哪个enclave,然后选择这个enclave中的哪个页。

### Q2: 选择是哪个enclave?

默认实现中，是一个轮转法。同时轮转进程（pid）和enclave

比如下图：

todo

第一次选择了pid 1里的第一个enclave,

第二次选择pid 2里的第一个enclave,

第三次选择pid 3里的第一个enclave,

第四次选择pid 4中的第二个enclave

既要轮转pid,又要轮转enclave,最后在enclave内轮转页。一共是三级轮转。
所以通过开另一个enclave,是很难影响别的enclave。因为别人牺牲一个页，你也要牺牲一个页

