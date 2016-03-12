# Effects-of-Inclusion-in-Hierarchical-Caches
Modified the Simplescalar simulator to implement the inclusion property in the L1 and L2 data caches. The inclusion property means that all the data in the L1 data cache (D-cache) are present in the unified L2 cache

DescriptionIn
this project, I modified the SimpleScalar simulator to implement the
inclusion properly in the L1 and L2 data caches. The inclusion property
means that all the data in the L1 data cache (D-cache) are present in the
unified L2 cache.
I took three benchmarks (Gcc, Vpr, Bzip2) and simulated them with three
different cache configurations. Observed the miss rates for IL1, DL1, and
combined L2 cache when running the three benchmarks on three cache
configurations before and after inclusion.
For implementing inclusion –
• We introduced a new parameter “cache_t”. This parameter is
passed when the “cache_create” function is called during the
execution.
• “NULL” value is passed whenever any other cache than L2 calls
the “cache_create”, else the address of L1 is passed.
• After “repl_addr” has been created in the function “cache_access”,
the function before performing any action, checks for the value and
then performs the following actions respectively-
• If address of L1 is set, then the address of L1 is passed to the
function “cache_flush_addr” for removal of address from
L1.
• Else if NULL value is set, it does not perform any action.
Result Collection –
For comparing the miss rates for IL1, DL1 and combined L2 cache when
running before and after inclusion, I noted miss rates for each of the
cache configuration and then plotted graphs showing the miss rates
before and after inclusion.

For implementing inclusion in Simplescalar we are required to
load/remove data from L1 cache whenever it is loaded/removed from L2
cache. So to implement this concept I passed an extra parameter
“cache_t” whenever “cache_create” is called. This change is made in
cache.c. Then I checked for the data in L1 whenever there is a cache miss
in L2. So basically I modified the cache_access() function because all the
read/write operations are performed through it. A parameter is added
which tracks the Loading/Removing in L1 cache with a condition of
accessing L2 cache. After the condition is satisfied it passes the address
of L1 if the address of L1 is set and if the NULL value is set is doesn’t
performs any action. In other words, if the value is found in L1, address is
passed to the function “cache_flush_addr” and it gets flushed and if it is
not found no action is performed. So if the other caches are accessed it
passes NULL value but if the Level 2 cache is accessed it passes Level 1
address.
(All the changes in code are marked by “ENF-INC-V_S”)
CONFIGURATION – A
Inclusion means whenever there is a removal in L2 cache, that particular
line should be removed from L1 also. Although, DL2’s cache line is
double of DL1 but associativity of DL2 is 8, higher than that of DL1.
Hence no change in miss rate for IL1.
CONFIGURATION - B
For this configuration, for every eviction in L2 cache, corresponding lines
should be removed in L1 and cache line of DL1 is half of the DL2 and
the DL2’s associativity is 2 which is lesser than DL1’s.
CONFIGURATION – C
For this configuration, for every eviction in L2 cache, corresponding lines
should be removed in L1 and cache line of DL1 is half of the DL2 and
the DL2’s associativity is 2 which is lesser than DL1’s. Therefore there is
a very slight increase in miss rate. But a drop in miss rate at combined L2
with Bzip2 is seen, as it is a cache capacity sensitive benchmark.
