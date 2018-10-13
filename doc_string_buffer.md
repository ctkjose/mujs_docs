
## Dynamic String Buffer ##

Defined in `jsi.h` and implemented in `jsintern.c`.

The type `js_Buffer *` allows you to implement a string buffer that is grown dynamically.

```c

js_Buffer *sb = NULL;

js_puts(js, &sb, "Hello world");
js_putc(js, &sb, ':');

js_free(js, sb);
```

Use `js_putm` to copy a portion of a string.

```c
js_Buffer *sb = NULL;

const char source = "Hello Jose";
const char *found;

found = strstr(source, "jose");

js_putm(js, sb, source, found)

```
