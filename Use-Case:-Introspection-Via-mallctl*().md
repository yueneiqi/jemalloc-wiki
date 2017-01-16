Compile the following code as such:

```sh
cc ex_mallctl.c -o ex_mallctl -I${JEMALLOC_PATH}/include -L${JEMALLOC_PATH}/lib \
-Wl,-rpath,${JEMALLOC_PATH}/lib -ljemalloc
```

ex_mallctl.c:

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
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
        size_t i, sz;

        for (i = 0; i < 100; i++) {
                do_something(i);

                // Update the statistics cached by mallctl.
                uint64_t epoch = 1;
                sz = sizeof(epoch);
                mallctl("epoch", &epoch, &sz, &epoch, sz);

                // Get basic allocation statistics.  Take care to check for
                // errors, since --enable-stats must have been specified at
                // build time for these statistics to be available.
                size_t sz, allocated, active, metadata, resident, mapped;
                sz = sizeof(size_t);
                if (mallctl("stats.allocated", &allocated, &sz, NULL, 0) == 0
                    && mallctl("stats.active", &active, &sz, NULL, 0) == 0
                    && mallctl("stats.metadata", &metadata, &sz, NULL, 0) == 0
                    && mallctl("stats.resident", &resident, &sz, NULL, 0) == 0
                    && mallctl("stats.mapped", &mapped, &sz, NULL, 0) == 0) {
                        fprintf(stderr,
                            "Current allocated/active/metadata/resident/mapped: %zu/%zu/%zu/%zu/%zu\n",
                            allocated, active, metadata, resident, mapped);
                }
        }

        return (0);
}
```

The output of the program will look like:

```text
Current allocated/active/metadata/resident/mapped: 181152/225280/1317632/1363968/4194304
Current allocated/active/metadata/resident/mapped: 192352/253952/1317632/1392640/4194304
Current allocated/active/metadata/resident/mapped: 214752/282624/1317632/1421312/4194304
...
Current allocated/active/metadata/resident/mapped: 1081056/1220608/1317632/2359296/4194304
Current allocated/active/metadata/resident/mapped: 1081056/1220608/1317632/2359296/4194304
Current allocated/active/metadata/resident/mapped: 1081056/1220608/1317632/2359296/4194304
```

The [jemalloc(3)](http://jemalloc.net/jemalloc.3.html) manual page documents all the mallctl names that are supported by a full-featured jemalloc build.  Additionally, jemalloc itself implements `malloc_stats_print()` in terms of `mallctl*()`, so an example of nearly all mallctl-related use cases can be found in the jemalloc source code itself ([`jemalloc/src/stats.c`](https://github.com/jemalloc/jemalloc/blob/master/src/stats.c)).