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

## Document map

The reference is split into small, testable pages. Each file is focused and self-contained.

- `docs/reference/overview.md` - Core model: attribute-first design and host responsibilities.
- `docs/reference/core-concepts.md` - Data wrapping, scope, expression evaluation, and interpolation.
- `docs/reference/directives.md` - All directives, grouped by category, with precise behavior.
- `docs/reference/lifecycle.md` - Initialization, update scheduling, rendering, and finalization.
- `docs/reference/communication.md` - `*fetch`, `*post`, `*api`, `*upload`, `*download`, and `*into`.
- `docs/reference/websocket.md` - `*websocket` and `*ws-send`, state keys, and events.
- `docs/reference/world.md` - World isolation and prefix-based composition.
- `docs/reference/advanced.md` - AST hooks, filters, methods injection, and warning/error events.

## Target source

This reference tracks the canonical code designated as [NABLLA_CANONICAL] in this repository as of 2025-10-31. When the canonical source changes, the reference will update with diffs aligned to that version.

## Conventions

- Hyphens are used instead of long dashes in English text.
- Examples prefer minimal HTML that can run without external tooling.
- Each page starts with a short “What you will find” section to set expectations.
