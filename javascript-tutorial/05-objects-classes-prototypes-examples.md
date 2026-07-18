# 05 - Objects, Classes, and Prototypes Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const user = { name: "Ada" };` | Creates an object literal. |
| 2 | `user.name` | Reads a property with dot notation. |
| 3 | `user["name"]` | Reads a property with bracket notation. |
| 4 | `user.email = "a@example.com";` | Adds or updates a property. |
| 5 | `delete user.email;` | Removes a property. |
| 6 | `"name" in user` | Checks whether a property exists. |
| 7 | `Object.keys(user)` | Gets property names. |
| 8 | `Object.values(user)` | Gets property values. |
| 9 | `Object.entries(user)` | Gets key/value pairs. |
| 10 | `{ ...user, active: true }` | Creates a shallow copy with changes. |
| 11 | `const { name } = user;` | Destructures a property. |
| 12 | `const { name: displayName } = user;` | Destructures and renames. |
| 13 | `class User {}` | Declares a class. |
| 14 | `constructor(name) { this.name = name; }` | Initializes a class instance. |
| 15 | `new User("Ada")` | Creates a class instance. |
| 16 | `static create() {}` | Declares a static class method. |
| 17 | `class Admin extends User {}` | Creates a subclass. |
| 18 | `super(name)` | Calls the base class constructor. |
| 19 | `Object.getPrototypeOf(user)` | Inspects an object's prototype. |
| 20 | `user instanceof User` | Checks whether an object was created through a constructor chain. |
