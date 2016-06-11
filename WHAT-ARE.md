# What are ES modules?

ES modules describe a couple of things:

* New **keywords** for using interfaces from other modules and presenting
  public interfaces from your modules: `import`, `export`.
* New **language rules** that allow modules to expose live bindings as
  interface, which solves neatly solves the issue of circular dependencies.
* **You don't have to type `"use strict"` anymore.** It's automatic, making
  your JavaScript faster and safer *by default*.

ES modules pave the cowpath made by Node.js modules. Here is what they look
like:

```javascript
// main.js
import {hello} from "./greetings"

hello("🌎")

// greetings.js
export function hello (target) {
  console.log(`hello ${target}`)
}
```

For an exhaustive look at what modules are and can do, [this Mozilla Developer
Network
article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
provides a deep dive on the topic.

Because both Node.js modules and ES modules are intended to solve the same
problem, integrating the new syntax into Node.js has run into some problems.

In particular, Node.js needs to identify whether a given file is a module of
the ES or Node.js variety before running it.
