# EPC页的access pattern 能不能被恶意的os监听到？

首先页的访问模式主要与page table有关。与tlb也有关系，但是考虑到tlb不命中(在实验中，命中率很低)，还是要回落到page table.所以只要看page table就行。 

然后SGX也是走tlb的。正是因为sgx走tlb,所以epcm里面会记录很多信息，sgx为了解决address translation attack做了很多设计 [sgx_explain]

os是能监控page table的，从 Controlled-Channel Attacks: Deterministic Side Channels for Untrusted Operating Systems这篇文章推测出来的。

综上：EPC页的access pattern能被恶意的os监听到

如果能SGX里能利用huge page,提高tlb命中率，能缓解这个问题。毕竟只要不经过page table,只经过tlb,就问题不大。
