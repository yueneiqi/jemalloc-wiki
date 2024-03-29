In addition to [leak checking at exit](https://github.com/jemalloc/jemalloc/wiki/Use-Case:-Leak-Checking), jemalloc can be told dump a profile:
* Approximately every so many bytes of allocation.  See the [`prof_leak`](http://jemalloc.net/jemalloc.3.html#opt.prof_leak) and [`lg_prof_interval`](http://jemalloc.net/jemalloc.3.html#opt.lg_prof_interval) [`MALLOC_CONF`](http://jemalloc.net/jemalloc.3.html#tuning) options.
* Every time the total virtual memory in use reaches a new high.  See the [`prof_gdump`](http://jemalloc.net/jemalloc.3.html#opt.prof_gdump) [`MALLOC_CONF`](http://jemalloc.net/jemalloc.3.html#tuning) option.
* Manually, via the ["prof.dump"](http://jemalloc.net/jemalloc.3.html#prof.dump) mallctl.  Use function calls something like the following (with proper error checking, of course).
  - Dump to specified filename.  Set `prof` to `true` in your [`MALLOC_CONF`](http://jemalloc.net/jemalloc.3.html#tuning):

    ```sh
    export MALLOC_CONF="prof:true,prof_prefix:jeprof.out"
    ```

    Then execute code similar to the following C code:

    ```c
    const char *fileName = "heap_info.out";
    mallctl("prof.dump", NULL, NULL, &fileName, sizeof(const char *));
    ```

  - Use automatic filename generation.  Set `prof_prefix` in your [`MALLOC_CONF`](http://jemalloc.net/jemalloc.3.html#tuning) in addition to `prof`:

    ```sh
    export MALLOC_CONF="prof:true,prof_prefix:jeprof.out"
    ```

    Then execute code similar to the following C code:

    ```c
    mallctl("prof.dump", NULL, NULL, NULL, 0);
    ```

`jeprof` can compare any two of the resulting series of profile dumps, and show what allocation activity occurred during the intervening time.  Use the `--base=<base>` flag to specify the first profile.

It is possible to start an application with profiling enabled but inactive, by specifying `MALLOC_CONF=prof_active:false`.  This is only useful if the application manually activates/deactivates profiling via the ["prof.active"](http://jemalloc.net/jemalloc.3.html#prof.active) mallctl during execution.  Use cases include:
* Activate profiling after initialization is complete, so that profiles only show objects allocated during steady-state execution.
* Dump a profile, activate profiling for 30 seconds, wait 30 seconds after deactivating profiling, then dump another profile and use `jeprof` to compare the two dumps.  This will focus on objects that were allocated during steady-state execution, but are long-lived.  These objects are prime candidates for explaining memory growth over time.

Walking the call stack to capture a backtrace is typically quite computationally intensive.  Therefore it is infeasible to use precise leak checking for long-lived, heavily loaded applications.  Statistical sampling of allocations makes it possible to keep the computational overhead low, yet get a general idea of how the application utilizes memory.  See the [`lg_prof_sample`](http://jemalloc.net/jemalloc.3.html#opt.lg_prof_sample) [`MALLOC_CONF`](http://jemalloc.net/jemalloc.3.html#tuning) option for information on controlling sampling interval.