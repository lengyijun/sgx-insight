# Enclave到底用了多少内存？

在编译enclave的时候，我们会看到这样的输出：

```
<EnclaveConfiguration>
<ProdID>0</ProdID>
<ISVSVN>0</ISVSVN>
<StackMaxSize>0x40000</StackMaxSize>
<HeapMaxSize>0x100000</HeapMaxSize>
<TCSNum>10</TCSNum>
<TCSPolicy>1</TCSPolicy>
<!-- Recommend changing 'DisableDebug' to 1 to make the enclave undebuggable for enclave release -->
<DisableDebug>0</DisableDebug>
<MiscSelect>0</MiscSelect>
<MiscMask>0xFFFFFFFF</MiscMask>
</EnclaveConfiguration>
tcs_num 10, tcs_max_num 10, tcs_min_pool 1
The required memory is 4063232B.
The required memory is 0x3e0000, 3968 KB.
Succeed.
SIGN =>  enclave.signed.so
The project has been built in debug hardware mode.
make[1]: Leaving directory '/home/lyj/linux-sgx/SampleCode/SampleEnclave
```

这里StackMaxSize是0x40_000, HeapMaxSize是0x100_000，加起来是 0x140_000

然而最后又说用了 0x3e0_000，大于 0x140_000,相差了0x2a0_000 （2688K）。这是因为代码本身的体积也要算进去，比如我这个enclave.signed.so的大小是401K,就算在这 2688K 中。还有些控制页也算进去了,比如VA,TCS.比如你把线程数少一个，内存就会小一点。

然后我们用sgxtop看输出，有一列是SIZE,他的大小是 16384K ，转换成16进制是 0x1_000_000,又大于 0x3e0_000，这是因为0x1_000_000 表明开辟的虚拟地址空间，必须是2的幂次。vm_area_struct



sgxtop还有个EADD的字段，大小是3968KB，转换成16进制是3e0_000。

所以 3e0_000 才是我们实际能用的空间，0x1_000_000这个字段毫无用处。

在运行时，SIZE不会变化，而EADD会变化

