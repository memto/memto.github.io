---
layout: post
ads: true
comments: true
published: true
title: Learning Python 5Ed
categories:
  - program
tags:
  - python
  - learning python
---
### Chapter 17: Scopes

##### namespace vs scope
_namespace_ - place where names live in. Python creates, changes, or looks up name in a namespace (name here can be variables' name, functions' name... etc).

_scope_ - name's visibility, it refers to a namespace. The location of a name's assignment determines the scope of the name's visibility.

_namespace_ and _scope_ seem a bit confuse here. but let say namespace as a box contains names: there is a _builtin namespace_ contains names like _open_, _getattr_, _setattr_ and so on; when in a module, there will be a _module namespace_ which contains global names from that module; when in a function call, there will a _function namespace_ contains names local to that funtion.

so, namespace is a box contains names. Scope defines which box you can see from parts of the program (that's why name's visibility). It defines where you could use a name without using any prefix.

In Python, almost everything related to names, including scope, happens at assignment time. Python uses the location of the assigment of a name to associate it with (_bind_ it to) a particular namespace. That is, the place of name assigment determines the _namespace it will live in_ and _its scope of visibility_.

Besides packaging code for reuse, functions add an extra namespace layer to minimize the potential for collisions among variables of the same name - by default, all names assigned inside a function are associated with that function’s namespace, and no other.

Three different scopes:
- If a variable is assigned inside a **def**, it is _local_ to that function.
- If a variable is assigned in an enclosing **def**, it is _nonlocal_ to nested functions.
- If a variable is assigned outside all **defs**, it is _global_ to the entire file.

>  This is _lexical scoping_ because variable scopes are determined entirely by the locations of the variables.

Any type of _assignment_ within a function classifies a name as local. This includes = statements, module names in **import**, function names in **def**, function argument names, and so on.

> **in-place changes** to objects do not classify names as locals; **only actual name assignments do**. <br>
For instance, if the name L is assigned to a list at the top level of a module, a statement L = X within a function will classify L as a local, but L.append(X) will not. In the latter case, we are changing the list object that L references, not L itself - L is found in the global scope as usual, and Python happily modifies it without requiring a global (or nonlocal) declaration. <br>
Distinction between **names** and **objects**: changing an object is not an assignment to a name.

##### Name Resolution: The LEGB Rule
- Name **assignments** create or change local names by default.
- Name **references** search at most four scopes: local, then enclosing functions (if any),
then global, then built-in.
- Names declared in _global_ and _nonlocal_ statements map assigned names to _enclosing module_ and _enclosing function_ scopes, respectively.

LGBT rules are applied for **references** or usage (not assignments) of simple variables

![python-legb.PNG]({{site.baseurl}}/media/python-legb.PNG)

##### Other Python scopes:
- Temporary variables in comprehension
- Exception reference variable in _try_ handlers
- Local scopes in _class_ statements

##### The built-in scope
The built-in scope is just a built-in module called ```builtins``` (3.x) or ```__builtin__``` (2.x)

The name ```__builtins__``` (with the s) is preset in most global scopes, including the interactive session, to reference the module known as ```builtins``` in 3.X and ```__builtin__``` in 2.X
![python-builtins.png]({{site.baseurl}}/media/python-builtins.png)

##### global and nonlocal
Concern with this only when do _assingments_ not _references_, references are simple follow LEGB rule.
![python-global-nonlocal.PNG]({{site.baseurl}}/media/python-global-nonlocal.PNG)

The 3.X nonlocal statement allow enclosing scope state changes. In 2.X, enclosing scopes are read-only.

##### Program design:
- Minimize Global Variables
- Minimize Cross-File Changes

#### Function factory or closure
_Closure_ is _functional programming_ technique in which nested function remember variables from its enclosing functions or environment (state retention).

#### State retention between function call
- using global variable
- class instance attribute
- using enclosing scope
- function default argument (with enclosing scope, it is not nescessery to use defaul argument to retain state between function call, but _loop variables_ still required)
- function attribute

### Chapter 18: Arguments

> Arguements are passed by reference

![python-arg-passing.PNG]({{site.baseurl}}/media/python-arg-passing.PNG)

In general, should avoid mutable argument changes
