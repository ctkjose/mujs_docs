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

**Example:**
```C
	js_getglobal(state, "name");
	if( js_isstring(state, -1) ){ //-1 the last value added to the value stack
		printf( "The global variable name has a value of %s", js_tostring(state, -1) );
	}
```

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

### Getting a C value. ###

Once a value is available and referenced in the Value Stack you can use one of the **GET** function to get a **C** primitive value.

| Function | Description |
| -- | -- |
| `int js_toboolean(js_State *J, int idx)` | Returns integer 0 or 1 |
| `double js_tonumber(js_State *J, int idx)` |  |
| `int js_tointeger(js_State *J, int idx)` |  |
| `int js_toint32(js_State *J, int idx)` |  |
| `unsigned int js_touint32(js_State *J, int idx)` |  |
| `short js_toint16(js_State *J, int idx)` |  |
| `unsigned short js_touint16(js_State *J, int idx)` | Returns an unsigned short |
| `const char *js_tostring(js_State *J, int idx)` |  Returns a CHAR * |

## Running javascript code ##

Using `js_loadeval` or `js_loadstring` you can run code in the global scope.

The functions `js_loadeval` or `js_loadstring` do not actually run your code.

They create a new `function` (js_Function) and push it to the top of the value stack.

We use `js_call` to execute the function in the stack.

To use `js_call` you have to push additional values to the stack
```c
const char *code = "var n = \"Jose\";";
js_loadeval(js, code); /*this is similar to eval*/
js_pushundefined(js); /*push value of this*/
js_call(js, 0); /*currently the function is on top of stack*/
```

### Adding a variable to the current scope ###

```c
#define SCOPE_OBJECT_CURRENT(js) (js->E->variables)
#define SCOPE_OBJECT_GLOBAL(js) (js->GE->variables)

js_pushstring(js,"Jose Cuevas");
jsR_defproperty(js, SCOPE_CURRENT(js), "name", JS_DONTENUM | JS_DONTCONF, stackidx(js, -1), NULL, NULL);

const char *code = "n = name;";
js_loadeval(js, code); /*this is similar to eval*/
js_pushundefined(js); /*push value of this*/
js_call(js, 0); /*currently the function is on top of stack*/

```

### Creating a Scope ###

```c
#define SCOPE_CURRENT(js) (js->E)
#define SCOPE_GLOBAL(js) (js->GE)

js_Object *private_code;
private_code = jsV_newobject(js, JS_COBJECT, NULL);

js_Environment *parent_scope;
js_Environment *scope;

parent_scope = SCOPE_CURRENT(js);

scope = jsScopeCreate(js, private_code, parent_scope);

jsScopePush(js, scope); /*set our new Scope

/*... code executed under new scope */

jsScopeRestore(js); /* exit our scope */

```
