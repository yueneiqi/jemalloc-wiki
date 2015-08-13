Run the `ex_stats_print` program shown on the [Getting Started wiki page](Getting-Started):

```sh
./ex_stats_print
```

Something like the following output will result:

```text
___ Begin jemalloc statistics ___
Version: 3.6.0-366-g85ae064e96387347915973dcc1ac15e553259bc9
Assertions disabled
Run-time option settings:
  opt.abort: false
  opt.lg_chunk: 21
  opt.dss: "secondary"
  opt.narenas: 16
  opt.lg_dirty_mult: 3 (arenas.lg_dirty_mult: 3)
  opt.stats_print: false
  opt.junk: "false"
  opt.quarantine: 0
  opt.redzone: false
  opt.zero: false
  opt.tcache: true
  opt.lg_tcache_max: 15
  opt.prof: false
  opt.prof_prefix: "jeprof"
  opt.prof_active: true (prof.active: false)
  opt.prof_thread_active_init: true (prof.thread_active_init: false)
  opt.lg_prof_sample: 19
  opt.prof_accum: false
  opt.lg_prof_interval: -1
  opt.prof_gdump: false
  opt.prof_final: false
  opt.prof_leak: false
CPUs: 4
Arenas: 16
Pointer size: 8
Quantum size: 16
Page size: 4096
Min active:dirty page ratio per arena: 8:1
Maximum thread-cached size class: 32768
Chunk size: 2097152 (2^21)
Allocated: 55410400, active: 59056128, metadata: 2861824, resident: 61739008, mapped: 65011712
Current active ceiling: 60817408

arenas[0]:
assigned threads: 1
dss allocation precedence: secondary
min active:dirty page ratio: 8:1
dirty pages: 14418:0 active:dirty, 0 sweeps, 0 madvises, 0 purged
                            allocated      nmalloc      ndalloc    nrequests
small:                        1781472          786            0           96
large:                       53628928          858            0          858
huge:                               0            0            0            0
total:                       55410400         1644            0          954
active:                      59056128
mapped:                      62914560
metadata: mapped: 1597440, allocated: 180352
bins:           size ind    allocated      nmalloc      ndalloc    nrequests      curregs      curruns regs pgs  util       nfills     nflushes      newruns       reruns
                   8   0          800          100            0            0          100            1  512   1 0.195            1            0            1            0
                     ---
                 112   7        11200          100            0            0          100            1  256   7 0.390            1            0            1            0
                 128   8          256            2            0            2            2            1   32   1 0.062            0            0            1            0
                     ---
                 224  11        22400          100            0            0          100            1  128   7 0.781            1            0            1            0
                     ---
                 320  13        20480           64            0            0           64            1   64   5 1                1            0            1            0
                     ---
                 448  15        28672           64            0            0           64            1   64   7 1                1            0            1            0
                 512  16         5120           10            0            0           10            2    8   1 0.625            1            0            2            0
                 640  17        20480           32            0            0           32            1   32   5 1                1            0            1            0
                 768  18        12288           16            0            0           16            1   16   3 1                1            0            1            0
                 896  19        43904           49            0           17           49            2   32   7 0.765            1            0            2            0
                1024  20        10240           10            0            0           10            3    4   1 0.833            1            0            3            0
                1280  21        20480           16            0            0           16            1   16   5 1                1            0            1            0
                1536  22        15360           10            0            0           10            2    8   3 0.625            1            0            2            0
                1792  23        28672           16            0            0           16            1   16   7 1                1            0            1            0
                2048  24        20480           10            0            0           10            5    2   1 1                1            0            5            0
                2560  25        25600           10            0            0           10            2    8   5 0.625            1            0            2            0
                3072  26        30720           10            0            0           10            3    4   3 0.833            1            0            3            0
                3584  27        35840           10            0            0           10            2    8   7 0.625            1            0            2            0
                4096  28        40960           10            0            0           10           10    1   1 1                1            0           10            0
                5120  29       189440           37            0           27           37           10    4   5 0.925            2            0           10            0
                6144  30        61440           10            0            0           10            5    2   3 1                1            0            5            0
                7168  31        71680           10            0            0           10            3    4   7 0.833            1            0            3            0
                8192  32        81920           10            0            0           10           10    1   2 1                1            0           10            0
               10240  33       307200           30            0           20           30           15    2   5 1                3            0           15            0
               12288  34       245760           20            0           10           20           20    1   3 1                2            0           20            0
               14336  35       430080           30            0           20           30           15    2   7 1                3            0           15            0
large:          size ind    allocated      nmalloc      ndalloc    nrequests      curruns
               16384  36       327680           20            0           20           20
               20480  37       839680           41            0           41           41
               24576  38      1007616           41            0           41           41
               28672  39      1204224           42            0           42           42
               32768  40      1343488           41            0           41           41
               40960  41      3358720           82            0           82           82
               49152  42      4079616           83            0           83           83
               57344  43      4702208           82            0           82           82
               65536  44      5373952           82            0           82           82
               81920  45     13434880          164            0          164          164
               98304  46     16121856          164            0          164          164
              114688  47      1835008           16            0           16           16
                     ---
huge:           size ind    allocated      nmalloc      ndalloc    nrequests   curhchunks
                     ---
--- End jemalloc statistics ---
```
The output is pretty self-explanatory if you have read the [jemalloc(3)](http://www.canonware.com/download/jemalloc/jemalloc-latest/doc/jemalloc.html) manual page.  Historically, the purpose of such statistics was to aid understanding of what jemalloc is actually doing internally in response to an application's memory allocation activity, in order to improve the allocator.  In practice the statistics are also useful to application developers; they give a strong indication of what jemalloc is up to, in terms of system calls, current memory usage, fragmentation, etc.  If the allocator is behaving unexpectedly, browse the stats for an obvious cause before resorting to more sophisticated analyses.

Note that by default jemalloc uses thread-local storage for object caching, and the statistics are only merged to the arena statistics counters whenever the arena has to be locked for some reason besides statistics gathering.  Thus these counters tend to be slightly out of date, but the inaccuracies are proportionally very small for non-toy applications that have been running for more than a short time.  That said, jemalloc does merge all statistics when the application exits.  Run the application as such to cause final statistics to be printed:

```sh
MALLOC_CONF=stats_print:true ./ex_stats_print
```

These statistics are accurate (as long as other threads aren't allocating during application exit).