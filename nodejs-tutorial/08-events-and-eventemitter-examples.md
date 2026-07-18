# 08 - Events and EventEmitter Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `new EventEmitter()` | Creates an event emitter. |
| 2 | `emitter.on("ready", handler)` | Subscribes to event. |
| 3 | `emitter.emit("ready")` | Publishes event. |
| 4 | `emitter.once("ready", handler)` | Runs listener once. |
| 5 | `emitter.off("ready", handler)` | Removes listener. |
| 6 | `emitter.removeListener(name, handler)` | Older removal method. |
| 7 | `emitter.listenerCount("ready")` | Counts listeners. |
| 8 | `emitter.eventNames()` | Lists event names. |
| 9 | `error event` | Special event that must be handled. |
| 10 | `events.once(emitter, "ready")` | Promise helper for one event. |
| 11 | `events.on(emitter, "data")` | Async iterator over events. |
| 12 | `MaxListenersExceededWarning` | Warning for likely listener leak. |
| 13 | `request event` | HTTP server receives requests as events. |
| 14 | `data event` | Streams emit data chunks. |
| 15 | `end event` | Stream finished. |
| 16 | `close event` | Resource closed. |
| 17 | `domain event` | App-specific event name. |
| 18 | `event payload` | Data passed to listeners. |
| 19 | `loose coupling` | Publisher does not need direct subscriber reference. |
| 20 | `avoid event spaghetti` | Too many events can obscure control flow. |
| 21 | `emitter.prependListener(name, handler)` | Adds listener before existing listeners. |
| 22 | `emitter.removeAllListeners(name)` | Removes all listeners for an event. |
| 23 | `emitter.setMaxListeners(20)` | Changes listener warning threshold. |
| 24 | `captureRejections` | Option that routes rejected async listeners to errors. |
| 25 | `EventTarget` | Web-standard event API also available in Node. |
| 26 | `AbortSignal` | EventTarget-based cancellation signal. |
| 27 | `listener order` | Listeners usually run in registration order. |
| 28 | `synchronous emit` | `emit` calls listeners synchronously. |
| 29 | `async listener` | Listener may start async work, but `emit` does not await it. |
| 30 | `domain-specific events` | Events named around app actions such as `order.created`. |

## Interview Questions

**Q1. What is `EventEmitter` used for?**  
It lets objects publish named events and lets other code subscribe to those events.

**Q2. What is special about the `error` event?**  
If an `error` event is emitted without a listener, Node can throw and crash the process.

**Q3. Are `emit` listeners called synchronously or asynchronously?**  
Listeners are called synchronously in registration order, unless the listener itself starts async work.

**Q4. When can events make code harder to maintain?**  
When too much business flow is hidden behind loosely connected events, making cause and effect hard to trace.

**Q5. What is `once` useful for?**  
It registers a listener that should run only the first time an event occurs.
