# ES Module Attributes

Champions: Sven Sauleau ([@xtuc](https://github.com/xtuc)), Daniel Ehrenberg ([@littledan](https://github.com/littledan)), Myles Borins ([@MylesBorins](https://github.com/MylesBorins)), and Dan Clark ([@dandclark](https://github.com/dandclark))

Status: Stage 1

The ES Module Attributes proposal is an investigation into providing inline syntax for module import statements to pass on more information alongside the module specifier, with an initial aim to support non-JS ESM module types.

## Motivation

As one example of an application: standards-track JSON ESM modules were [proposed](https://github.com/w3c/webcomponents/issues/770) to allow JavaScript modules to easily import JSON data files, similarly to how they are supported in many nonstandard JavaScript module systems. This idea quickly got broad support from web developers and browsers, and was merged into HTML, with an implementation created by Microsoft.

However, in [an issue](https://github.com/w3c/webcomponents/issues/839), Ryosuke Niwa (Apple) and Anne van Kesteren (Mozilla) proposed that security would be improved if some syntactic marker were required when importing JSON modules and similar module types which cannot execute code, to prevent a scenario where the responding server unexpectedly provides a different MIME type, causing code to be unexpectedly executed. The solution was to somehow indicate that a module was JSON, or in general, not to be executed, somewhere in addition to the MIME type.

Some developers have the intuition that the file extension could be used to determine the module type, as it is in many existing non-standard module systems. However, it's a deep web architectural principle that the suffix of the URL (which you might think of as the "file extension" outside of the web) does not lead to semantics of how the page is interpreted. In practice, on the web, there is a widespread [mistmatch between file extension and the HTTP Content Type header](content-type-vs-file-extension.md). All of this sums up to it being infeasible to depend on file extensions/suffixes included in the module specifier to be the basis for this checking.

There are other possible pieces of metadata which could be associated with modules, see [#8](https://github.com/littledan/proposal-module-attributes/issues/8) for futher discussion.

Proposed ES module types that are blocked by this security concern, in addition to JSON modules, include [CSS modules](https://github.com/whatwg/html/pull/4898) and potentially [HTML modules](https://github.com/whatwg/html/pull/4505) if the HTML module  proposal is restricted to [not allow script](https://github.com/w3c/webcomponents/issues/805).

## Rationale

There are three places where this data could be provided:
- As part of the module specifier (e.g., as a pseudo-scheme)
  - Challenges: Adds complexity to URLs or other module specifier syntaxes, and risks being confusing to developers (further discussion: [#11](https://github.com/littledan/proposal-module-attributes/issues/11))
- Separately, out of band (e.g., a separate resource file)
  - Challenges: How to load that resource file; what should the format be; unergonomic to have to jump between files during development (further discussion: [#13](https://github.com/littledan/proposal-module-attributes/issues/13))
- In the JavaScript source text
  - Challenges: Requires a change at the JavaScript language level (this proposal)

This proposal pursues the third option, as we expect it to lead to the best developer experience, and are hopeful that language design/standardization issues can be resolved.

## Early draft syntax

Module attributes have to be made available in several different contexts. This section contains one possible syntax, but there are other options, discussed in [#6](https://github.com/littledan/proposal-module-attributes/issues/6).

Here, a single string is permitted to describe a single module attribute, as discussed in [#12](https://github.com/littledan/proposal-module-attributes/issues/12).

Although unspecified in the module attributes proposal, this intention of the proposal champions is that this string would be interpreted as the module type, as shown in the following examples.

The JavaScript standard would basically be responsible for passing the string up to the host environment, which could then decide how to interpret it. Issues [#24](https://github.com/littledan/proposal-module-attributes/issues/24) and [#25](https://github.com/littledan/proposal-module-attributes/issues/25) discuss the Web and Node.js feature and semantic requirements respectively, and issue [#10](https://github.com/littledan/proposal-module-attributes/issues/10) discusses how to allow different JavaScript environments to have interoperability.

### import statements

The ImportDeclaration would allow a string provided at the end of an import statement with the `as` keyword.

```js
import json from "./foo.json" as "json";
```

The host environment would determine the interpretation of `"json"`. In the Web and similar environments, `as "json"` would be required to load JSON modules, whereas no `as` syntax would be used for JavaScript modules.

### dynamic import()

The `import()` pseudo-function would allow the string as its second argument.

```js
import("foo.json", "json")
```

### Integration of modules into environments

Host environments (e.g., the Web platform, Node.js) often provide various different ways of loading modules. The analogous string could be passed through these ways of loading other kinds of modules.

#### Worker instantiation

```js
new Worker("foo.wasm", { as: "webassembly" });
```

Sidebar about WebAssembly module types and the web: it's still uncertain whether importing WebAssembly modules would need to be marked specially, or would be imported just like JavaScript. Further discussion in [#19](https://github.com/littledan/proposal-module-attributes/issues/19).

#### HTML

```html
<script src="foo.wasm" type="webassembly"></script>
```

(See the caveat about WebAssembly above.)

#### WebAssembly

In the context of the [WebAssembly/ESM integration proposal](https://github.com/webassembly/esm-integration): For imports of other module types from within a WebAssembly module, this proposal would introduce a new custom section (named `importattributes`) that will annotate with attributes each imported module (which is listed in the import section).

## Status and plan for standardization

This proposal is at Stage 1.

Standardization here would consist of building consensus not just in TC39 but also in WHATWG HTML as well as the Node.js ESM effort and a general audit of semantic requirements across various host environments ([#10](https://github.com/littledan/proposal-module-attributes/issues/10), [#24](https://github.com/littledan/proposal-module-attributes/issues/24) and [#25](https://github.com/littledan/proposal-module-attributes/issues/25)). Stage 2 will also require that we have a strong initial answer for the surface syntax ([#6](https://github.com/littledan/proposal-module-attributes/issues/6)), including the choice of whether we use a single string or key/value pairs ([#12](https://github.com/littledan/proposal-module-attributes/issues/12)).

Please leave any feedback you have in the [issues](http://github.com/littledan/proposal-module-attributes/issues)!

### Anticipated follow-on proposal: Richer `as` values

In a possible follow-on proposal, other kinds of literals such as object literals, array literals or numbers may be permitted following `as`, if they only consist of constants with no need for runtime evaluation. For example:

```js
import value from "module" as {key1: "value1", key2: [1, 2, 3]};
```

This would allow module attributes to scale up to multiple pieces of data, not privileging a single attribute forever. Due to the lack of strong use cases, we've left this capability for a follow-on proposal.
