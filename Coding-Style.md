jemalloc's coding style is based on [FreeBSD's style(9)](https://www.freebsd.org/cgi/man.cgi?query=style&sektion=9), but with enough deviations to warrant stand-alone documentation.  The following applies to C/C++ files; for other languages such as POSIX shell, make a best effort to mimic existing jemalloc code.

C code conforms to the C99 standard.

Tabs stops are 8 characters apart.
```
0       8      16      24      32      40      48      56      64      72
|       |       |       |       |       |       |       |       |       |
```

Lines are wrapped to be no more than 80 characters wide, with continuation lines indented an additional 4 characters.  Exceptions are primarily due to single tokens that are too far indented and/or too long to be able to follow this rule.
```C
/*
0       8      16      24      32      40      48      56      64      72      80
|       |       |       |       |       |       |       |       |       |       X
*/
                                        some_func(with, several, arguments,
                                            that, do, not,
                                            fit_on_a_single_line, and,
                                            a_symbol_that_cannot_follow_the_rule);                                                 
```

Tab characters are never immediately preceded by space characters.  Code blocks are tab-indented one level deeper than their enclosing blocks.
```C
/*
0       8      16      24      32      40      48      56      64      72      80
|       |       |       |       |       |       |       |       |       |       X
*/
void
foo(int x)
{
        int x;

        for (i = 0; i < x; i++) {
                if (bar(i))
                        return;
        }    
}
```

[XXX Much more to come.]