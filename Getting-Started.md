All of the following examples assume a standard developer environment on Unix-like machine, and something like the following shell environment variable configuration:

```sh
export JEMALLOC_PATH=$HOME/local
```

There are several ways to integrate jemalloc into an application. Here are some examples, from simplest to most involved:

* Use the LD_PRELOAD environment variable to inject jemalloc into the application at run time. Note that this will only work if your application does _not_ statically link a malloc implementation.

  ```sh
  LD_PRELOAD=${JEMALLOC_PATH}/lib/libjemalloc.so.1 app
  ```

* Link jemalloc into the application at build time, but use it as a generic malloc implementation:

  ```sh
  cc app.c -o app -L${JEMALLOC_PATH}/lib -Wl,-rpath,${JEMALLOC_PATH}/lib -ljemalloc
  ```

* Compile jemalloc with an API prefix (see the `--with-jemalloc-prefix` configure option), link with jemalloc at build time as above, but use jemalloc distinctly from the system allocator.

Once you have jemalloc integrated into your application, you can use special features in a variety of ways:

* Set the `/etc/malloc.conf` symlink or [`MALLOC_CONF`](http://www.canonware.com/download/jemalloc/jemalloc-latest/doc/jemalloc.html#tuning) environment variable to tune jemalloc, e.g.

  ```sh
  export MALLOC_CONF="prof:true,lg_prof_sample:1,prof_accum:false,prof_prefix:jeprof.out"
  ```

* Directly invoke jemalloc features in the application:
  - Compile the following code as such:

    ```sh
    gcc ex_stats_print.c -o ex_stats_print -I${JEMALLOC_PATH}/include \
    -L${JEMALLOC_PATH}/lib -Wl,-rpath,${JEMALLOC_PATH}/lib -ljemalloc
    ```

  - ex_stats_print.c:
    ```c
    #include <stdlib.h>
    #include <jemalloc/jemalloc.h>

    void
    do_something(size_t i)
    {

            // Leak some memory.
            malloc(i * 100);
    }

    int
    main(int argc, char **argv)
    {
            size_t i;

            for (i = 0; i < 1000; i++) {
                    do_something(i);
            }

            // Dump allocator statistics to stderr.
            malloc_stats_print(NULL, NULL, NULL);

            return (0);
    }
    ```