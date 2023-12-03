比如在 32 位 cpu 中，内存地址的范围是：`0x00000000 - 0xffffffff`，也就是 `4GB`

Linux 内核将这些内存按照 3/1（2/2，或者 1/3 都行）的比例分割成 user space（highmem） 和 kernel space（lowmem）

