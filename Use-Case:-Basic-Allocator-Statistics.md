Run the `ex_stats_print` program shown on the [Getting Started wiki page](Getting-Started):

```sh
./ex_stats_print
```

Something like the following output will result:

```text
___ Begin jemalloc statistics ___
Version: 3.4.1-10-g96eeaec5dd5ac4d11be36945240fa823abc0c3f9
Assertions disabled
Run-time option settings:
  opt.abort: false
  opt.lg_chunk: 22
  opt.dss: "secondary"
  opt.narenas: 128
  opt.lg_dirty_mult: 3
  opt.stats_print: false
  opt.junk: false
  opt.quarantine: 0
  opt.redzone: false
  opt.zero: false
  opt.tcache: true
  opt.lg_tcache_max: 15
  opt.prof: false
  opt.prof_prefix: "jeprof"
  opt.prof_active: true
  opt.lg_prof_sample: 19
  opt.prof_accum: false
  opt.lg_prof_interval: -1
  opt.prof_gdump: false
  opt.prof_final: true
  opt.prof_leak: false
CPUs: 32
Arenas: 128
Pointer size: 8
Quantum size: 16
Page size: 4096
Min active:dirty page ratio per arena: 8:1
Maximum thread-cached size class: 32768
Chunk size: 4194304 (2^22)
Allocated: 52919712, active: 52940800, mapped: 62914560
Current active ceiling: 54525952
chunks: nchunks   highchunks    curchunks
             15           15           15
huge: nmalloc      ndalloc    allocated
            0            0            0

arenas[0]:
assigned threads: 1
dss allocation precedence: secondary
dirty pages: 12925:0 active:dirty, 0 sweeps, 0 madvises, 0 purged
            allocated      nmalloc      ndalloc    nrequests
small:        1015200          969            0            0
large:       51904512          965            0          965
total:       52919712         1934            0          965
active:      52940800
mapped:      54525952
bins:     bin  size regs pgs    allocated      nmalloc      ndalloc    nrequests       nfills     nflushes      newruns       reruns      curruns
            0     8  501   1          800          100            0            0            1            0            1            0            1
[1..6]
            7   112   72   2         8064           72            0            0            1            0            1            0            1
[8..10]
           11   224   72   4        16128           72            0            0            1            0            1            0            1
[12]
           13   320   63   5        20160           63            0            0            1            0            1            0            1
[14]
           15   448   63   7        28224           63            0            0            1            0            1            0            1
           16   512   63   8        32256           63            0            0            1            0            1            0            1
           17   640   51   8        32640           51            0            0            1            0            1            0            1
           18   768   47   9        36096           47            0            0            1            0            1            0            1
           19   896   45  10        40320           45            0            0            1            0            1            0            1
           20  1024   63  16        64512           63            0            0            1            0            1            0            1
           21  1280   51  16        65280           51            0            0            1            0            1            0            1
           22  1536   42  16        64512           42            0            0            1            0            1            0            1
           23  1792   38  17        68096           38            0            0            1            0            1            0            1
           24  2048   65  33       133120           65            0            0            1            0            1            0            1
           25  2560   52  33       133120           52            0            0            1            0            1            0            1
           26  3072   43  33       132096           43            0            0            1            0            1            0            1
           27  3584   39  35       139776           39            0            0            1            0            1            0            1
large:   size pages      nmalloc      ndalloc    nrequests      curruns
         4096     1            5            0            5            5
         8192     2           41            0           41           41
        12288     3           41            0           41           41
        16384     4           41            0           41           41
        20480     5           41            0           41           41
        24576     6           41            0           41           41
        28672     7           41            0           41           41
        32768     8           42            0           42           42
        36864     9           41            0           41           41
        40960    10           41            0           41           41
        45056    11           41            0           41           41
        49152    12           41            0           41           41
        53248    13           41            0           41           41
        57344    14           41            0           41           41
        61440    15           41            0           41           41
        65536    16           41            0           41           41
        69632    17           41            0           41           41
        73728    18           41            0           41           41
        77824    19           41            0           41           41
        81920    20           41            0           41           41
        86016    21           41            0           41           41
        90112    22           41            0           41           41
        94208    23           41            0           41           41
        98304    24           41            0           41           41
       102400    25           16            0           16           16
[991]
--- End jemalloc statistics ---
```
The output is pretty self-explanatory if you have read the [jemalloc(3)](http://www.canonware.com/download/jemalloc/jemalloc-latest/doc/jemalloc.html) manual page.  Historically, the purpose of such statistics was to aid understanding of what jemalloc is actually doing internally in response to an application's memory allocation activity, in order to improve the allocator.  In practice the statistics are also useful to application developers; they give a strong indication of what jemalloc is up to, in terms of system calls, current memory usage, fragmentation, etc.  If the allocator is behaving unexpectedly, browse the stats for an obvious cause before resorting to more sophisticated analyses.

Note that by default jemalloc uses thread-local storage for object caching, and the statistics are only merged to the arena statistics counters whenever the arena has to be locked for some reason besides statistics gathering.  Thus these counters tend to be slightly out of date, but the inaccuracies are proportionally very small for non-toy applications that have been running for more than a short time.  That said, jemalloc does merge all statistics when the application exits.  Run the application as such to cause final statistics to be printed:

```sh
MALLOC_CONF=stats_print:true ./ex_stats_print
```

These statistics are accurate (as long as other threads aren't allocating during application exit).