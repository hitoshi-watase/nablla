# Filters

_What you will find:_ normative definitions of built-in filters, their invocation points, lookup order, return type contracts, and error behavior. No tutorials are included.

## Purpose

Filters transform values at fixed points in the rendering and I/O pipeline. Implementations ship defaults and allow per-world overrides.

## Registration and lookup

- Filters are resolved by this order:
  1) world registry
  2) global layer
  3) implementation defaults
- Replacing a filter affects only the world where it is registered unless an explicit global registration API is used.
- Registration API names are implementation-defined.

## Invocation points

The runtime calls filters at these points:

- Text interpolation `%expr%`: `text`, `placeholder`
- Output
  - `*print`: `text`
  - `*compose`: `html`
- Attributes
  - dynamic attribute values: `attr`
  - `:style` or `n-style`: `style`
- Input binding
  - to data: `input_in`
  - from data to control: `input_out`
- URL-like normalization where applicable: `url`

## Built-in filters

### `text(value) -> string`
- Converts any value to the string used for text nodes or text-like attributes.
- Must not insert HTML. Use `html` for markup.
- Nullish handling is implementation-defined but must be stable within a version.

### `html(value) -> string`
- Converts any value to an HTML string used by `*compose`.
- The filter is the hook point for sanitization. Behavior is implementation-defined but must be documented by the implementation.

### `placeholder(value) -> string`
- Produces display text for placeholders and similar UI hints.
- Often delegates to `text` with different nullish rules.

### `attr(name, value) -> string | null`
- Produces the final attribute string for a dynamic attribute.
- Returning `null` removes the attribute.
- `name` is the attribute name being set.

### `style(value) -> string`
- Accepts a CSS string or an object map `{ prop: value }`.
- Returns a CSS text string suitable for `style` attribute.
- Implementations define normalization rules for units, kebab-casing, and priorities.

### `input_in(control, value) -> any`
- Converts a control’s raw value to the data representation.
- `control` is the originating element.
- Must be pure with respect to the provided inputs.

### `input_out(control, value) -> any`
- Converts a data value to a control’s view representation.
- `control` is the destination element.
- Must be pure with respect to the provided inputs.

### `url(value) -> string`
- Normalizes URL-like values for HTTP directives when applicable.
- Exact normalization rules are implementation-defined.

## Return type contracts

- `text`, `html`, `placeholder`, `style`, `url` return strings.
- `attr` returns a string or `null`.
- `input_in` and `input_out` return any JSON-serializable value or a type the implementation supports for control state.

## Error handling

- Exceptions thrown by filters do not crash the host by default. The runtime logs a warning or error and falls back to a safe value when possible.
- If a filter returns a type outside its contract, the runtime treats it as a no-op or coerces to a safe default.

## Stability rules

- Filters are called deterministically for a given input and invocation point in a single render pass.
- Implementations may cache results across a pass. Filters must be side-effect free.

## World scope

- Overrides are scoped to the registering world.
- Cross-world behavior does not share filter instances or state.

---
Back to index: [`README.md`](./README.md)
