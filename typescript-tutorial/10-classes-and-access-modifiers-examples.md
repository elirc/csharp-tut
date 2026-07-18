# 10 - Classes and Access Modifiers Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `class User {}` | Declares a class. |
| 2 | `constructor(name: string) {}` | Typed constructor parameter. |
| 3 | `public name: string` | Public property. |
| 4 | `private token: string` | Compile-time private member. |
| 5 | `protected id: string` | Available to subclasses. |
| 6 | `readonly id: string` | Assignable only during initialization. |
| 7 | `#secret` | JavaScript runtime-private field. |
| 8 | `constructor(public id: string) {}` | Parameter property. |
| 9 | `class Admin extends User {}` | Class inheritance. |
| 10 | `super(name)` | Calls base constructor. |
| 11 | `implements Logger` | Class promises to satisfy interface. |
| 12 | `abstract class Shape` | Base class that cannot be directly instantiated. |
| 13 | `abstract area(): number` | Subclass must implement method. |
| 14 | `static create()` | Static class method. |
| 15 | `get fullName()` | Getter property. |
| 16 | `set fullName(value)` | Setter property. |
| 17 | `override method()` | Explicitly marks an override. |
| 18 | `strictPropertyInitialization` | Checks class fields are initialized. |
| 19 | `dependency injection` | Constructor receives collaborators. |
| 20 | `prefer interfaces for contracts` | Keeps dependencies flexible. |
