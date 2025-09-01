# Dispach Table in C

We can make a dispatch table just using preprocessor macros in C. Simulated TBB behavior if you are familiar with ARM ASM.

This is example code from a lab I did in school:

We first define a macro which is a list of names we want to have a string association with a particular func. The `X()` wrapper allows us to define the `X` macro and call the NAME_LIST macro to change the behavior of the wrapper. For example:

```C
#define NAME_LIST \
    X(name1)    \
    X(name2)    \
    ...
    X(nameN)
```

Will define the `NAME_LIST`. Then we will create a function prototype for each name in `NAME_LIST` by defining `X` as a macro. We then want to call `#undef` so we can reuse the `NAME_LIST` macro.

```C
#define X(NAME) void print_NAME();
NAME_LIST
#undef X
```

We can then define some more macros:

```C
#define FUNC_NAME(name) <function prefix>_##name_<function suffix>
#define FUNC_SIGNATURE(func_name) uint16_t func_name(uint16_t arg1, char *arg2, char *arg3, char *arg4, char *arg5, char *arg6)
#define FUNC_PROTO(name) FUNC_SIGNATURE(FUNC_NAME(name))
```

And then define our dispatch table by first defining an entry in the table:

```C
typedef struct dispatch_table_entry {
    const char *name;
    FUNC_SIGNATURE((*func)); // This macro is expanded to a function pointer with the signature defined above
} dispatch_table_entry;
```

We can generate the function prototypes with:

```C
#define X(NAME) FUNC_PROTO(NAME);
NAME_LIST
#undef X
```

And finally, create a dispatch table which corresponds names with functions:

```C
static const dispatch_table_entry dispach_table[] = {
#define X(NAME) {#NAME, FUNC_NAME(NAME)},
    NAME_LIST
#undef X
};
const int num_names = sizeof(dispach_table) / sizeof(dispatch_table_entry);
```

Finally, we just need to make the function definitions for each named function.

```C
FUNC_PROTO(name1) {
    ...
}

...

FUNC_PROTO(nameN) {
    ...
}
```

Pretty dope IMO.
