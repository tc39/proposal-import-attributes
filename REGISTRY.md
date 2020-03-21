# Module attributes registry

A central goal of this proposal is to have consistent behavior across environments, to the maximum extent possible. See [README.md](./README.md) for more context. This document attempts to be the basis for a registry where attribute names and semantics can be standardized across environments.

NOTE: The idea of a registry like this is a very early idea for discussion; it doesn't have consensus yet, and we'd be open discussing alternatives.

This registry is intended to be inclusive of all JavaScript environments and tooling, coordinating among the whole ecosystem. The current draft includes things in this proposal and proposals for HTML, but the Web is not intended to be treated specially. For example, we may decide on coordinating on an attribute which is useful across multiple tools.

## Attribute names

### `type`

- **Standards status/breadth-of-support**: Part of the Stage 1 module attributes proposal itself at TC39
- **Summary of semantics**: The `type` attribute enables non-JavaScript module types by providing a secondary place where the module type is indicated.
- **Caching behavior**: Not part of the cache key, just a check. JavaScript implementations are required to return the same module based on the specifier and other keys, or an error; they may not reinterpret the module based on the type.

### Other ideas

Further attribute key ideas in [#8](https://github.com/tc39/proposal-module-attributes/issues/8).

## `type` values

### `json`

- **Standards status/breadth-of-support**: Part of the Stage 1 JSON modules and module attributes proposal itself at TC39. In previous iterations at WHATWG, JSON modules had multiple implementer support.
- **Summary of semantics**: A single default export of the parsed JSON module, or an error if the type does not match the host-defined expectations for JSON (e.g., on the Web, a JSON MIME type).

### Other ideas

#### `html`

- **Standards status/breadth-of-support**: Under early discussion at [W3C Web Components](https://github.com/w3c/webcomponents/issues/645)
- **Summary of semantics**: Various alternatives under discussion; could be an inert DOM tree, or could be a more full-featured declarative component system.

#### `css`

- **Standards status/breadth-of-support**: Under early discussion at [W3C Web Components](https://github.com/w3c/webcomponents/issues/759).
- **Summary of semantics**: In the first iteration, a detached stylesheet of the indicated CSS file. Ongoing discussion about the semantics of `@import`.

#### `webassembly`

- **Standards status/breadth-of-support**: [WebAssembly/ESM integration](https://github.com/webassembly/esm-integration) is Phase 2 in WebAssembly CG. [Discussion](https://github.com/w3c/webcomponents/issues/839) about pros and cons of requiring an explicit `type` for WebAssembly modules.
- **Summary of semantics**: WebAssembly/ESM integration treats module specifiers and named imports within WebAssembly as ESM imports, and treats exports as ESM exports. WebAssembly has its own MIME type, which HTML would check when loading a WebAssembly module.

## What should I do if I want to use an attribute not stated here?

In the module attributes proposal, attributes or `type` values which are not properly registered here are required to be rejected by conforming JavaScript implementations. That means, the module graph would not load. This includes all implementations which intend to be standards-compliant, whether browsers, tools, server or embedded environments, etc. Unrecognized module attributes must not simply be ignored (we understand, however, that there may be implementations of proposed standards).

We invite you all to participate in the development of this registry, discussing module attributes with the broader community, so that we can ensure compatibility across environments for use of module attributes. This registry may be extended to include some attributes which make sense only in certain environments, and not others. For example, a potential future `type` value may be `"html"`, which likely doesn't make sense in every JavaScript environment, but could make sense in multiple JavaScript implementations.

## Standards track

This registry is not a standard. The plan is to put it on a standards track sooner rather than later, with a cadence related to how the module attributes proposal moves through the TC39 stage process.
- As a Stage 2 prerequisite, TC39 would need to come to consensus on the general approach of having such a registry for module attributes.
- As a Stage 3 prerequisite, TC39 must sign off on the registry's proposed process, for how things are introduced, agreed-on and change over time.
- As a Stage 4 prerequisite, this registry must have a proposed home in a standards body, as Stage 4 would come with archiving this repository. Before Stage 4

### Strawperson process

(Very much open to change! Not looking for committee consensus on this until Stage 3 at the earliest.)

- This registry has an editor or editor group, appointed by <insert parent standards body>.
- PRs are generally welcome in the "other ideas" category; it's a sort of Stage 0, and leaves the attribute forbidden per spec. To get something merged at this point, it's best to have a complete description of semantics and support, but standardization/broad buy-in is not required.
- When multiple implementations or environments support an attribute, or when it's a (proposed?) standard, and a PR has passed registry editor (group) review, then it may be merged. (Details pending for what "multiple" means; this is a bit subtle.)
- Depending on the parent standards organization, the process for actually "forming a standard" may be different, but either way, in practice, merging a PR would be "standard enough" for real use:
    - If in TC39, then this would be another annually released document; we'd encourage people to look at the editor's draft for the most up-to-date copy, as we do for the ECMA-262 standard.
    - If in WHATWG, this would be a living standard.
    - If in W3C, it would ideally be according to the new proposed process for registries.
    - This could also just be a GitHub repository somewhere.
- While the module attributes proposal is under development, and before Stage 4, the champion group would play this role of the registry editor group.

