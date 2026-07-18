# 08 - Events and EventEmitter

Many Node APIs are event-based. `EventEmitter` lets objects publish named events.

```js
import { EventEmitter } from "node:events";

const bus = new EventEmitter();

bus.on("task:created", (task) => {
  console.log(task.title);
});

bus.emit("task:created", { title: "Learn Node" });
```

Events are useful for notifications and integration points. They can also make flow harder to follow if overused.

Always handle `error` events on emitters that may emit them. An unhandled `error` event can crash the process.

## Exercises

1. Create an `EventEmitter`.
2. Register an event listener.
3. Emit an event with data.
4. Register a one-time listener.
5. Handle an `error` event.
