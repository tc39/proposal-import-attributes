# ES module attributes

Proposal for syntax to import ES modules with attributes

## Motivation

https://github.com/w3c/webcomponents/issues/839

### When would the checks happen

On the web, before any code executes, check that the mimetype matches what type was provided explicitly.

## Integrations

### import statements

```js
import json from "./foo.json" with type: "json";
```

### dynamic import()

```js
import("foo.json", { type: "json" })
```

### HTML

```html
<script src="foo.wasm" type="webassembly"></script>
```

### WebAssembly

Custom section: per import specifier, what options are needed

### Worker instantiation

```js
new Worker("foo.wasm", { type: "webassembly" });
```

### Node.js

```sh
node --experimental-modules --input-type=webassembly foo.wasm
```
