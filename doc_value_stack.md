## Virtual Value Stack ##

**MuJS** uses a **virtual stack** to pass values to and from C. Each element in this stack represents a Javascript value (null, number, string, etc).




Whenever Javascript calls C, the called function gets a new stack. This stack initially contains the this value and any arguments passed to the function. When the C function returns, the top value on the stack is passed back to the caller as the return value.

### Accessing values in the stack. ###

There are generally two types of operations with the stack we can **PUSH** a value or **GET** a value.

The values are accessed using stack indices. The first value in the stack is index **0**.
(*We said that index 0 is the TOP of the stack.*)

When executing in the context of a **function** called from JS the index 0 contains the `this` value, and function arguments are index 1 and up.

You can also use negative indexes. A Negative index references a value in the stack from the opposite direction. An index of value -1 is the last value in the stack and index -2 is the one prior to the last.

Think of it as index -1 to be the value last referenced by **GET**.

### Getting a value from the GLOBAL scope ###

MuJS provides multiple functions to get values. The most basic one is the function `js_getglobal`.

`void js_getglobal(js_State *state, const char *name)`

The function `js_getglobal` takes as arguments the `js_State` and the name of the symbol that we want to find in the global scope.



js_getglobal(state, "name");

### Testing the type of a value in the stack. ###


| Function | Description |
| -- | -- |
| `int js_isdefined(js_State *J, int idx)` | Returns 1 if value is not undefined |
| `int js_isundefined(js_State *J, int idx)` | Returns 1 if value is is undefined |
| `int js_isnull(js_State *J, int idx)` | Returns 1 if value is is NULL |
| `int js_isboolean(js_State *J, int idx)` | Returns 1 if value is is boolean |
| `int js_isnumber(js_State *J, int idx)` | Returns 1 if value is numeric |
| `int js_isstring(js_State *J, int idx)` | Returns 1 if value is string |
| `int js_isprimitive(js_State *J, int idx)` | Returns 1 if value is boolean, numeric, string |
| `int js_isobject(js_State *J, int idx)` | Returns 1 if value is an object |
| `int js_isarray(js_State *J, int idx)` | Returns 1 if value is an array |
| `int js_isregexp(js_State *J, int idx)` | Returns 1 if value is a RegExp |
| `int js_iscallable(js_State *J, int idx)` | Returns 1 if value can be executed |
| `int js_iscoercible(js_State *J, int idx)` | Returns 1 if value can be coerced |
| `int js_iserror(js_State *J, int idx)` | Returns 1 if value is an error object |
| `int js_isuserdata(js_State *J, int idx)` | Returns 1 if value is an object with user data |
