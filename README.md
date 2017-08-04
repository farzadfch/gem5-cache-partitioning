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


### Limitations
