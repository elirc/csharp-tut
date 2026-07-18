# 05 - Objects, Classes, and Prototypes

Objects group related data and behavior.

```js
const user = {
  id: 1,
  name: "Ada",
  greet() {
    return `Hi, ${this.name}`;
  }
};
```

Use dot notation for normal property access and bracket notation when the property name is dynamic.

```js
user.name;
user["name"];
```

Classes are syntax over JavaScript's prototype system. They are useful when you need repeated object shapes with shared methods.

```js
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hi, ${this.name}`;
  }
}
```

Prototypes are how JavaScript shares behavior between objects. You do not need to manually edit prototypes often, but you should understand that class methods live on the prototype.

## Exercises

1. Create an object with properties and a method.
2. Use a dynamic property name.
3. Write a class with a constructor and method.
4. Inspect an object's prototype in the console.
