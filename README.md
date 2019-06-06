## Gem5 L2 Cache Partitioning

This patch adds L2 cache partitioning feature to gem5. Cache partitioning assigns a subset of cache ways to each core such that a core is limited to its assigned subset of ways for allocating lines in the cache. In order to enable cache partitioning, the function **m5_enablewaypart** must be called from the program running in the simulated machine with a non-zero integer passed to it as the argument. Example:

```
#include  "m5op.h"

int main()
{
     // enable cache partitioning
     m5_enablewaypart(1);
}
```

Passing zero to this function disables cache partitioning. Partitioning is only implemented for LRU replacement therefore, the cache must be configured to use the LRU tag in order for partitioning to be enabled.

The patch can be directly applied to revision 10422 of gem5. I believe it is not very hard to manually apply the modifications to a later version of gem5. The ways assigned to each core are hard coded in the function **allocateBlock** in *src/mem/cache/cache_impl.hh*. In order to find which CPU has requested to allocate a line in L2, the code looks for the CPU names in *MasterName*. This way, the CPU names in the Python script must be the same as the constant CPU strings defined in **allocateBlock**. I think it should be possible to make the code more flexible by passing the partitions configuration and CPU *MasterName*s from the Python script to the C++ code and avoiding any hard coded constant.
