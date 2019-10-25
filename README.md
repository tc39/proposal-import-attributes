# ES module attributes

Proposal for syntax to import ES modules with attributes

Proposed by [littledan](https://github.com/littledan) and [xtuc](https://github.com/xtuc).

## Motivation

https://github.com/w3c/webcomponents/issues/839

"The author is the only true arbiter of the parse goal of content" -- Jordan H.

More information about the possible [mistmatch between file extension and the HTTP Content Type header](content-type-vs-file-extension.md).

### Semantics

On the web, before any code executes this semantics apply before any code executes.

A possible extension of this proposal would be to allow to specifiy a SRI hash in module's attributes.

#### `type`

Check that the ressource has the expected type using its mimetype.

## Integrations

### import statements

The ImportDeclaration would allow any arbitrary attributes after the `with` keyword.

A `type` key should be mandatory to load a module that isn't JavaScript (json, css, html, ...).

```js
import json from "./foo.json" with type: "json";
```

### dynamic import()

The `import` function will allow to provide arbritrary attributes as its second arguments.

```js
import("foo.json", { type: "json" })
```

### HTML

```html
<script src="foo.wasm" type="webassembly"></script>
```

WebAssembly's validation will fail if the magic number is not correct.
We'd assert that the mimetype matches the type before even before running the WebAssembly validation.

### WebAssembly

Introduce a new custom section (named `importattributes`) that will annotate with attributes each imported module (based on the import section).

### Worker instantiation

```js
new Worker("foo.wasm", { type: "webassembly" });
```

### Tooling

Tooling could take advantage of the module attributes to generate better code or add additional features.

### Node.js

```sh
node --experimental-modules --input-type=webassembly foo.wasm
```
