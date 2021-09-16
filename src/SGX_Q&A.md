# 内存

## Q1：当一个EPC页从DRAM交换回EPC的时候，他还会到原先的物理页位置吗？

不会

driver会找到一个空闲的EPC页分配给他。找到哪个就是哪个了



## Q2: sdk与psw有什么区别？

sdk是给enclave（安全区）用的

psw是给app（非安全区）用的



## Q3： 为什么要用修改过的as ld ld.gold？ 源码在哪里？

不知道。找不到源码



## Q4: 一个进程需要哪些信息才能找到enclave？

只需要虚拟地址就够了

 ### Q5: 进程如何通过虚拟地址找到enclave？

先找到vma。

vma->private_data 



## Q6： 当发生缺页中断的时候，OS怎么知道这个中断需要交给sgx的page fault hanlder处理，而不是OS自己的？

vma中有一个结构体，指向了自定义的函数。由于缺的页的地址落在这个vma里面，就会调用这个vma里的函数指针



## Q7: 当一个EPC page交换出去之后，交换到外面的地址是哪里？记录在哪里？

有一个get_backing的函数，能在内核中分配可以交换到磁盘的内存页（一般kernel的页是不会交换到磁盘的）

能根据虚拟地址快速定位到backing page的位置

## Q*8: if epc page is swapped out, what happens to the page table entry?
I guess no change happened.(I'm not sure)

