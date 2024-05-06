# C++

- [Keywords](#keywords)
- [C# differences](#c-differences)
- [Templates](#templates)
- [Operator overloading](#operator-overloading)
- [Move Semantics](#move-semantics)


## Keywords

- extern: used in a var declaration to indicate the value is defined elsewhere
- mutable: indicates the member var may be changed even in const contexts
- static: allocate space for the lifetime of the program
  - if the variable is at namespace scope, it can't be accessed from any other translation unit. Use to prevent global namespace pollution
- const
  - variables: the value can't change
  - member function: the member variables can't change
  - When left of *, the pointed to value can't change
  - When right of *, the pointer itself can't change
- final (actually a "declarator attribute"); used to prevent subclassing a class or overriding a virtual function


[Top](#c)

## C# differences

- Objects (and structs) are value types. Declaring a variable for one constructs it on the stack immediately
- There are no interfaces, just abstract classes, for which there is no keyword- simply make the class's methods pure virtual (=0)
- Classes don't have visibility modifiers
- Multi-inheritance supported
- Use the class name to invoke parent functions

[Top](#c)

## Templates

- Template specializations are created by declaring a template with fewer, or more specific template parameters in the template expression. Removing all template parameters is called a full specialization (template<>).
- A class template specialization won't use declarations/definitions provided by the full template. (but global operator overloads will apply to both)
- nested dependent typenames (such as T::const_iterator) must be preceded by the keyword "typename" (otherwise the compiler assumes it's not a type, generating compilation errors), except when they appear in base class list or as a base class identifier in an initialization list.
- traits are pretty cool. They are simply templates which give you information about the templated type argument.
  
  These traits can contain typedefs for empty "tag" structs, Another template can then default construct the tag struct from the traits and pass to a function. With method overloads taking each possible "tag" struct type, this essentially gives you compile time if...else tests on types.
- Having a class C inherit from a template with a template parameter of C is called the "curiously recurring template pattern" (CRTP).

[Top](#c)

## Operator overloading

- =, [], (), new, and delete overloads must be member functions. Others can be global or member. Prefer to make global

## Move semantics

### r values and l values

- https://learn.microsoft.com/en-us/cpp/cpp/lvalues-and-rvalues-visual-cpp?view=msvc-170

rvalues indicate objects eligible for move operations, while lvalues generally don't. A useful heuristic to determine whether an expression is an lvalue is to ask if you can take its address. If you can, it typically is. If you can't, it's usually an rvalue.
The type of an expression is independent of whether the expression is an lvalue or an rvalue. Case in point, a parameter expression is always an lvalue (you can take its address), even when the parameter type is an rvalue reference. Parameters are lvalues, but the arguments with which they are initialized may be rvalues or lvalues.

To reiterate, an expression has a type as well as status as an lvalue or rvalue. Consider
```
int&& v = 42;
```
The expression 'v' is an lvalue- we can take its address. But the type of expression 'v' is int&&- an int rvalue.
When considering what function to call, the status as an lvalue or rvalue is used.
```
void foo(int&& x){}
void foo(int& x){}
foo(v);// invokes foo(int& x); arument is type int&&, but parameter is type int&
```
The argument 'v' is an rvalue, but the function called has parameter type lvalue. This is because the argument expression is an lvalue.

The argument type not matching the parameter type isn't so unusual.
```
void foo(int& x){}
int x = 42;
foo(x); // argument is type int, but parameter is type int&
```
### Understand std::move, and std::forward

"type&&" doesn't always represent an rvalue reference. When "type" is a template parameter that is an lvalue reference (eg, T = int&), then T&& becomes an lvalue reference.
To remove the "referenceness" of T to get int, we can use
```
remove_reference<T>:type
```
Then we can make that type into an rvalue reference by adding &&.
eg:
```
using ReturnType = typename remove_reference<T>::type&&; // C++11
using ReturnType = remove_reference_t<T>&&; // C++ 14
```
This is what std::move does to give you an rvalue from an lvalue.

An rvalue reference to a non-const cannot bind to a const rvalue (ie, can't pass const rvalue to move constructor). However,  an lvalue reference to a const can bind to a const rvalue (ie, you can pass const rvalue to copy constructor).
In other words, move requests on const objects are silently transformed into copy operations. This makes sense as a move request typically changes the moved from object.

std::move and std::forward are simply casts. They do nothing at runtime. They simply change the type, which affects which methods are chosen (at compile time). Whether the copy constructor or move constructor is called depends on the type of the right-hand-side.  So if you cast an l-value to an r-value, assigning from it will invoke the move constructor instead of the copy constructor.

Whereas std::move unconditionally casts to an rvalue, std::forward casts to an rvalue only if it had been passed in as an rvalue. But how does a function even take both l and r values? Through a template!
```
template<typename T>
void foo(T&& param){}
```
If T is int&, T&& becomes int&&& which becomes int& (I think). So types can be deduced to generate one function that takes a lvalue, and another function that takes an rvalue. So they're not really the same function.

### Distinguish universal references from rvalue referencs

Universal reference is a term coined by Scott Meyers.
They are called so because they can bind to virtually anything. lvalue/rvalue, const/non-const, volatile/non-volatile.

In code they appear like rvalue references (&&), but only in certain contexts. A good rule of thumb is if "T" is a deduced type, then "T&&" is a universal reference.
```
auto&& var2 = var1; // var2 is a universal reference
template <typename T> void f(T&& param); // param is a universal reference. Note it must be "T&&" exactly (eg, not "vector<T>&&", not "const T&&", etc).
```
Both of these contexts use "type deduction". So l-r/const/volatile may be deduced.
For template class member functions, the function template parameter must be independent of any class template parameters. Otherwise, there is no type deduction- the template argument is determined by the object created.

### Use std::move on rvalue references, std::forward on universal references

But only the last time you use the variable in the function

[Top](#c)
