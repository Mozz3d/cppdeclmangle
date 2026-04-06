# C++ Declaration Mangler — Usage Examples

Accepts one or more C++ declarations as quoted arguments and produces their
MSVC-style mangled names.

```
cppdeclmangle.py  "DECLARATION" ["DECLARATION" ...]
```

---

## Global functions

A plain free function with optional calling convention and parameters.

```
cppdeclmangle.py "int add(int, int)"
```
```
Mangling of :- "int __cdecl add(int,int)"
is :- "?add@@YAHH0@Z"
```

A function with a void return and parameters.

```
cppdeclmangle.py "void doNothing(void)"
```
```
Mangling of :- "void __cdecl doNothing(void)"
is :- "?doNothing@@YAXXZ"
```

A function taking a pointers and references to types and returning a void pointer.

```
cppdeclmangle.py "void* myFunction(class MyClass &, struct Ns::MyStruct *, unsigned char &, float*)"
```
```
Mangling of :- "void * __ptr64 __cdecl myFunction(class MyClass & __ptr64,struct Ns::MyStruct * __ptr64,unsigned char & __ptr64,float * __ptr64)"
is :- "?myFunction@@YAPEAXAEAVMyClass@@PEAUMyStruct@Ns@@AEAEPEAM@Z"
```

---

## Class methods

A public, void parameters, class method.

```
cppdeclmangle.py "public: void MyClass::myMethod(void)"
```
```
Mangling of :- "public: void MyClass::myMethod(void)"
is :- "?myMethod@MyClass@@QEAAXXZ"
```

A protected class method with parameters.

```
cppdeclmangle.py "protected: void MyClass::myMethod(class MyOtherClass *,float)"
```
```
Mangling of :- "protected: void MyClass::myMethod(class MyOtherClass *,float)"
is :- "?myMethod@MyClass@@IEAAXPEAVMyOtherClass@@M@Z"
```

A private `virtual` method with a `const` instance qualifier.

```
cppdeclmangle.py "private: virtual void MyClass::myVirtualMethod(void) const"
```
```
Mangling of :- "private: virtual void MyClass::myVirtualMethod(void) const"
is :- "?myVirtualMethod@MyClass@@EEBAXXZ"
```

A public `static` method.

```
cppdeclmangle.py "public: static int MyClass::myStaticMethod(void)"
```
```
Mangling of :- "public: static int MyClass::myStaticMethod(void)"
is :- "?myStaticMethod@MyClass@@SAHXZ"
```

A namespace qualified class and method.

```
cppdeclmangle.py "public: virtual void Ns::MyClass::myMethod(void) const"
```
```
Mangling of :- "public: virtual void Ns::MyClass::myMethod(void) const"
is :- "?myMethod@MyClass@Ns@@UEBAXXZ"
```

---

## Constructors and destructors

A constructor taking no parameters.

```
cppdeclmangle.py "public: MyClass::MyClass()"
```
```
Mangling of :- "public: MyClass::MyClass()"
is :- "??0MyClass@@QEAA@XZ"
```

A constructor taking two parameters.

```
cppdeclmangle.py "public: MyClass::MyClass(int, char)"
```
```
Mangling of :- "public: MyClass::MyClass(int, char)"
is :- "??0MyClass@@QEAA@HD@Z"
```

A destructor.

```
cppdeclmangle.py "public: MyClass::~MyClass(void)"
```
```
Mangling of :- "public: MyClass::~MyClass(void)"
is :- "??1MyClass@@QEAA@XZ"
```

A `virtual` destructor.

```
cppdeclmangle.py "public: virtual MyClass::~MyClass(void)"
```
```
Mangling of :- "public: virtual MyClass::~MyClass(void)"
is :- "??1MyClass@@UEAA@XZ"
```

---

## Static member variables

A public static data member. Variable declarations encode the access class and CV
qualifiers separately from function manglings.

```
cppdeclmangle.py "public: static int MyClass::myStaticMember"
```
```
Mangling of :- "public: static int MyClass::myStaticMember"
is :- "?myStaticMember@MyClass@@2HEA"
```

---

## Passing multiple declarations at once

```
cppdeclmangle.py \
  "public: MyClass::MyClass(int, char)" \
  "public: virtual MyClass::~MyClass()" \
  "public: static int MyClass::getCount(void)"
```
```
Mangling of :- "public: MyClass::MyClass(int, char)"
is :- "??0MyClass@@QEAA@HD@Z"

Mangling of :- "public: virtual MyClass::~MyClass(void)"
is :- "??1MyClass@@UEAA@XZ"

Mangling of :- "public: static int __cdecl MyClass::getCount(void)"
is :- "?getCount@MyClass@@SAHXZ"
```

---

## Supported declaration forms at a glance

| Form | Example |
|---|---|
| Global function | `int fn(int, char)` |
| Pointer return / parameter | `int * alloc(unsigned int)` |
| Public member function | `public: void Cls::method(void)` |
| Virtual const method | `public: virtual void Cls::method(float) const` |
| Static method | `public: static int Cls::method(void)` |
| Constructor | `public: Cls::Cls()` |
| Destructor | `public: virtual Cls::~Cls()` |
| Nested namespace method | `public: virtual void Ns::Cls::method(float) const` |
| Static data member | `public: static int Cls::member` |
| Elaborated type parameter | `private: virtual void Cls::handler(struct MyStruct *) const` |

---