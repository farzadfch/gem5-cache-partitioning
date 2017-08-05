## Gem5 L2 Cache Partitioning

This patch adds L2 cache partitioning feature to gem5. Cache partitioning assigns a subset of cache ways to each core such that each core is limited to its assigned subset for allocating lines in the cache. In order to enable cache partitioning, the function **m5_enablewaypart** must be called from the program running on top of the gem5 with a non-zero integer value passed to it as the argument. Example:

```
#include  "m5op.h"

int main()
{
     // enable cache partitioning
     m5_enablewaypart(1);
}
```

Passing zero to this function disables cache partitioning.

The patch can be directly applied to revision 10422 of gem5 (a relatively old version of gem5). I believe it is still not too much of a hurdle if someone wants to manually apply the modification to a later version of gem5. The ways assigned to each core are hard coded in file *src/mem/cache/cache_impl.hh* in function **allocateBlock**. In order to find who is requesting to allocate a line in L2, the code looks for the CPU name in the *MasterName*. I agree that it is a little bit dirty to find the requester CPU in this way. I am open to ideas to improve this section.
