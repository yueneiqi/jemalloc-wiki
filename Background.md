# History
jemalloc started out as the memory allocator for a programming language runtime Jason Evans was developing in 2005, but language design changes made the allocator superfluous. At the time, [FreeBSD](http://www.freebsd.org/) was in need of an SMP-scalable allocator, so he integrated jemalloc into FreeBSD's libc, and then made a long series of improvements to scalability and fragmentation behavior.

In late 2007, the [Mozilla Project](http://www.mozilla.org/) was hard at work improving [Firefox's](http://www.mozilla.com/firefox/) memory usage for the 3.0 release, and jemalloc was used to solve fragmentation problems for Firefox on [Microsoft Windows](http://www.microsoft.com/windows/) platforms. You can read [here](http://blog.pavlov.net/2008/03/11/firefox-3-memory-usage/) about the fruits of that labor. Many general jemalloc enhancements resulted from Firefox-related efforts. More recently, in 2010 Mozilla sponsored integration of [Apple Mac OS X](http://www.apple.com/macosx/) support into the stand-alone jemalloc, and in 2012 contributed [MinGW](http://www.mingw.org/) Windows support.

Starting in 2009, Jason Evans adapted jemalloc to handle the extreme loads [Facebook](http://www.facebook.com/) servers commonly operate under, and added numerous features that support development and monitoring. Facebook uses jemalloc in many components that are integral to serving its website, and as of 2017 a small Facebook team drives ongoing development and maintenance.

# Intended use
jemalloc is a general purpose ```malloc(3)``` implementation that emphasizes fragmentation avoidance and scalable concurrency support.  It is intended for use as the system-provided memory allocator, as in FreeBSD's libc library, as well as for linking into C/C++ applications.  jemalloc provides many introspection, memory management, and tuning features beyond the standard allocator functionality.  As an extreme example, arenas can be used as pool allocators; i.e. an arena can be used for general purpose allocation, and then the entire arena destroyed as a single operation.

# Adoption
Major uses of jemalloc include:
* [FreeBSD](http://www.freebsd.org/)
* [NetBSD](http://www.netbsd.org/)
* [Mozilla Firefox](http://www.mozilla.org/firefox/)
* [Varnish](https://www.varnish-cache.org/)
* [Cassandra](http://cassandra.apache.org/)
* [Redis](http://redis.io/)
* [hhvm](https://github.com/facebook/hiphop-php)
* [MariaDB](https://mariadb.org/)
* [Aerospike](http://www.aerospike.com/)
* [Android](https://github.com/android/platform_bionic)

# Additional documentation
The following documentation is dated or otherwise unsuited to wiki form:
* <a href="http://jemalloc.net/jemalloc.3.html">jemalloc(3) manual page</a>: The manual page for the latest release fully describes the API and options supported by jemalloc, and includes a brief summary of its internals.
* <a href="http://applicative.acm.org/2015/applicative.acm.org/speaker-JasonEvans.html">Applicative 2015 presentation</a>: This talk (<a href="https://youtu.be/RcWp5vwGlYU?list=PLn0nrSd4xjjZoaFwsTnmS1UFj3ob7gf7s">video</a>) presented the background and initial design ideas that lead to decay-based unused dirty page purging, first available in the <a href="https://github.com/jemalloc/jemalloc/releases/tag/4.1.0">4.1.0</a> release.
* <a href="http://www.facebook.com/notes/facebook-engineering/scalable-memory-allocation-using-jemalloc/480222803919">Facebook Engineering post</a>: This article was written in 2011 and corresponds to jemalloc 2.1.0.
* <a href="http://people.freebsd.org/~jasone/jemalloc/bsdcan2006/jemalloc.pdf">BSDcan paper</a>: This paper was presented at the <a href="http://www.bsdcan.org/">BSDcan conference</a> in 2006 and roughly corresponds to the version of jemalloc that appeared in <a href="http://www.freebsd.org/">FreeBSD</a> 7.
