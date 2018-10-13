## Values ##

A values is represented by `js_Value *`.

### Creating values. ###

| Function | Description |
| -- | -- |
| `js_Value jsValueInteger(js_State *js, int v)` | Creates a number value with an integer.  Does not implements the `math.prototype`. |
| `js_Value jsValueObjectNumber(js_State *js, double v)` | Value representing a number. Implements the `math.prototype`. |
| `js_Value jsValueString(js_State *js, const char *v)` | Value representing a string. Does not implements the `string.prototype`. |
| `js_Value jsValueObjectString(js_State *js, const char *v)` | Value representing a string object. Implements the `string.prototype`. |
| `js_Value jsValueObject(js_State *js, js_Object *prototype)` |  Returns a value with a pointer to an object in `value.u.object`. |


## Objects ##

Objects are represented by the struct `js_Object`.

### Create an object ###

We use the function `jsV_newobject` to create a representation of an object.

`js_Object *jsV_newobject(js_State *J, enum js_Class type, js_Object *prototype)`


```c
	/*no prototype*/
	js_Object *obj = jsV_newobject(js, JS_COBJECT, NULL);

```

Your js_State holds references to some of the built-in prototypes:

```c
js_Object *Object_prototype;
js_Object *Array_prototype;
js_Object *Function_prototype;
js_Object *Boolean_prototype;
js_Object *Number_prototype;
js_Object *String_prototype;
js_Object *RegExp_prototype;
js_Object *Date_prototype;
```

```c
	/* with default object.prototype */
	js_Object *obj = jsV_newobject(js, JS_COBJECT, js->Object_prototype);
```


## Memory and Garbage Collection ##

### Strings ###

To allow the runtime to manage references to strings use the function `js_intern` to get a managed string from a `const char *`.

```c
const char *js_intern(js_State *J, const char *s)
```


## Internal Methods ##

Defined in `jsvalue.c`;
