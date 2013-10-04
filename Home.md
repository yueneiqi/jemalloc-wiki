[jemalloc](http://www.canonware.com/jemalloc/) is a general purpose malloc implementation that is optimized for multi-threaded applications.  It also tends to have better memory fragmentation behavior than other implementations do.  jemalloc first came into use as FreeBSD's libc allocator in 2005, but it has also found its way into numerous applications, simply because other allocators were unable to handle the load placed on them.  Starting in 2010 jemalloc development efforts broadened to include developer support features such as heap profiling, [Valgrind](http://valgrind.org/) integration, and extensive monitoring/tuning hooks.  Modern jemalloc releases continue to be integrated back into [FreeBSD](http://www.freebsd.org/), so broad usefulness remains critical.  Put succinctly, jemalloc strives to perform miserably for as few real world applications as realistically possible, at the same time as being among the best allocators for a broad range of demanding applications.

Major users of jemalloc include:
* [FreeBSD](http://www.freebsd.org/)
* [NetBSD](http://www.netbsd.org/)
* [Mozilla Firefox](http://www.mozilla.org/firefox/)
* [Varnish](https://www.varnish-cache.org/)
* [Cassandra](http://cassandra.apache.org/)
* [Redis](http://redis.io/)
* [hhvm](https://github.com/facebook/hiphop-php)
* [MariaDB](https://mariadb.org/)

[Frequently Asked Questions](wiki/Frequently-Asked-Questions)