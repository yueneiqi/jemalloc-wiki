jemalloc's coding style is based on [FreeBSD's style(9)](https://www.freebsd.org/cgi/man.cgi?query=style&sektion=9), but with enough deviations to warrant stand-alone documentation.  The following applies to C/C++ files; for other languages such as POSIX shell, make a best effort to mimic existing jemalloc code.

C code conforms to the C99 standard.

Tabs stops are 8 characters apart, starting at column 0 and ending in principle after column 72.  Wrap lines to occupy only columns [0..80) whenever possible, with continuation lines indented an additional 4 columns.  Exceptions are primarily due to single tokens that are too far indented and/or too long to be able to follow this rule.  Try to avoid exceptions by limiting block nesting depth.  Compose whitespace for indentation as a sequence of 0 or more tab characters, followed by fewer than 8 space characters.  Increase block indentation by one tabstop for each major indentation level.
```C
void
foo(int x, some_type_t some_var_a,  some_type_t some_var_b,
    some_type_t some_var_c) {
        for (int i = 0; i < x; i++) {
                if (bar(i)) {
                        while (biz(i)) {
                                do {
                                        some_func(with, several, arguments,
                                            that, cannot, fit_on_a_single_line,
                                            and,
                                            a_symbol_that_can_not_follow_the_rule);
                                } while (baz(i));
                        }
                        return;
                }
        }  
}
/*------|-------|-------|-------|-------|-------|-------|-------|-------|-------
0       8      16      24      32      40      48      56      64      72     */
```

Align switch labels with the enclosing block.  Similarly, align goto labels to column 0.
```C
void
bar(int x) {
        switch (x) {
        case 0:
                if (do_something()) {
                        goto label_error;
                }
                /* Fall through. */
        case 1:
                do_more();
                break;
        default:
                not_reached();
        }

        do_yet_more();
        return;
label_error:
        recover_from_error();
}
/*------|-------|-------|-------|-------|-------|-------|-------|-------|-------
0       8      16      24      32      40      48      56      64      72     */
```

Write single-line comments with ```/*...*/``` delimiters, and put the delimiters on separate lines for multi-line comments.  Use complete sentences, with two spaces after periods.
```C
/* This is a single line comment. */

/*
 * This is a multi-line comment.  Follow sentence-ending periods with two
 * spaces.  Consider that we display code with fixed-width fonts; embrace the
 * old 20th-century convention and like it.
 */

/* (Yes, the following comment violates formatting convention.) */
/*------|-------|-------|-------|-------|-------|-------|-------|-------|-------
0       8      16      24      32      40      48      56      64      72     */
```

Enclose all blocks in braces.  Put opening braces on the same lines as associated control flow.  Put ```else``` on the same line as the preceding closing brace.
```C
void
f(void) {
        if (foo) {
        } else if (bar) {
        } else {
        }

        switch (foo) {
        case 0:
                break;
        case 1: {
                int x = a();
                do {
                        b();
                } while (x < c);
                break;
        } default:
                not_reached();
        }
}
```
[XXX Much more to come.]