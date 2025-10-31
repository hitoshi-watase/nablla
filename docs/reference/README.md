# Nablla Reference Documentation

This repository hosts the official reference for Nablla. It is written for developers browsing the project on GitHub who need precise and stable definitions of behaviors, attributes, and directives.

## Purpose

- Define what Nablla does at the attribute and lifecycle level.
- Specify each directive and its effect on data, attributes, and DOM.
- Describe the update cycle, events, and communication primitives in a testable way.
- Avoid tutorial flow. For step-by-step learning, see the separate Guide.

## What Nablla is

Nablla is a Web Components based library that treats HTML attributes as the source of truth. Browsers already propagate attributes to the DOM. Nablla complements this by driving the data → attributes direction so that attribute-first synchronization is consistent and explicit.

## Scope and audience

- Audience: developers reading the codebase, integrating Nablla into products, or evaluating its design.
- Scope: reference and normative descriptions. No debugging tips or performance advice.

## Terminology

- **Host element**: the custom element instance that runs Nablla, e.g. `<na-blla>`.
- **Data**: a JSON object bound to the host via the `data` attribute or by setting `host.data = {...}` at runtime. Nablla wraps it for reactivity.
- **Directive**: an attribute that instructs Nablla. There are two forms:
  - Asterisk form: `*if`, `*each`, `*let`, etc.
  - `n-` form: `n-if`, `n-each`, `n-let`, etc. Both forms are equivalent.
- **Shorthand attributes**: `:class`, `:style` for dynamic attributes.
- **Methods**: functions exposed to expressions via `*methods` or `n-methods`.
- **Lifecycle**: initialization and the update cycle that renders templates and applies side effects.
- **Into key**: the storage key designated by `*into` or `n-into` for placing results of network and file operations.

## Notation used in this document

- Code and attributes are shown inline as code literals, for example ``*if="count > 0"`` or block examples in fenced code.
- Text interpolation uses `%expr%`. Nablla evaluates the expression and inserts the result.
- Within expressions:
  - `el` refers to the current element in event handlers.
  - `$event` is the native DOM event object.
  - `scope` denotes the current Nablla data context.
- The host element is written as `<na-blla>` in examples.

## Index

- [Overview](./overview.md)
- [Core Concepts](./core-concepts.md)
- [Directives](./directives.md)
- [Lifecycle](./lifecycle.md)
- [Communication](./communication.md)
- [WebSocket](./websocket.md)
- [World](./world.md)
- [Host API](./host-api.md)
- [Expressions](./expressions.md)
- [Filters](./filters.md)
- [Events](./events.md)
- [Advanced](./advanced.md)
- [Configuration](./configuration.md)
- [Warnings and Errors](./warnings-and-errors.md)
- [Glossary](./glossary.md)
- [Reserved and Transient](./reserved-and-transient.md)
- [Grammar](./grammar.md)

## Target source

This reference tracks the canonical code designated as [NABLLA_CANONICAL] in this repository as of 2025-10-31. When the canonical source changes, the reference will update with diffs aligned to that version.

## Conventions

- Hyphens are used instead of long dashes in English text.
- Examples prefer minimal HTML that can run without external tooling.
- Each page starts with a short “What you will find” section to set expectations.
