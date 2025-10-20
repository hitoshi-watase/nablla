# Data Fundamentals

> Status: draft  
> Applies to: Nablla core, directive engine

## 1) Why (background)

Before learning bindings or events, authors need a clear, minimal mental model of **what counts as data**, **how to initialize it**, and **how changes are observed**. This chapter fixes those basics so later topics (bindings, scopes, worlds) remain predictable.

## 2) What this chapter covers (and defers)

Covers here:
- Allowed data shapes (primitives, objects, arrays, dates as strings, etc.)
- Initialization patterns (inline attribute / scripted)
- Mutation rules and change detection
- Ownership and persistence guidelines
- Common pitfalls (serialization, huge payloads, cycles)

Deferred to later chapters:
- **Bindings** `:prop`, `@event`, and `%%` text expansion → Chapter 4  
- **Scopes & Worlds** (resolution order, visibility) → Chapter 5

## 3) Allowed data shapes

Nablla is optimized for **plain JSON-like structures**.

- **Primitives**: `string`, `number`, `boolean`, `null`
- **Objects**: plain objects (no class instances required)
- **Arrays**: any element types above
- **Dates**: use ISO strings (e.g., `"2025-10-20T04:00:00Z"`) or numbers (epoch ms).  
  If you need real `Date` instances, initialize via script (see 4.2).
- **Unsupported / discouraged inside inline JSON**: functions, symbols, `Map/Set`, circular references, DOM nodes, class instances?serialize them to plain data or set them via script.

Rule of thumb: **Prefer small, flat, descriptive objects**. Deeply nested graphs are harder to reason about and cost more to diff.

## 4) Initialization

### 4.1 Inline (attribute)

Use the `data` attribute with a JSON string. Keep it compact and HTML-safe.

```html
<my-nablla id="app" data='{"title":"Hello Nablla","items":[1,2,3]}'></my-nablla>
```

Notes
- Attribute value must be valid JSON (double-quoted keys/strings).  
- Escape quotes as needed, or prefer small payloads to avoid complex escaping.
- For large/remote data, hydrate later (see 6.2).

### 4.2 Scripted (programmatic)

When JSON is inconvenient (dates, large objects, or computed data), set via script.

```html
<my-nablla id="app"></my-nablla>
<script>
  const app = document.getElementById("app");
  // Safe: serialize first when using setAttribute
  app.setAttribute("data", JSON.stringify({ title: "Hello", count: 0 }));

  // Or, if your build exposes a public API:
  // app.data = { title: "Hello", createdAt: new Date().toISOString() };
</script>
```

Notes
- Prefer `setAttribute("data", JSON.stringify(obj))` for maximum portability.
- If a public property API is available (e.g., `el.data = {...}`), prefer ISO strings for dates to keep SSR/rehydration simple.

## 5) Mutation & change detection

Nablla tracks changes on **objects/arrays** within the component’s data.

- **Object fields**: `obj.title = "New"` → observed.
- **Array ops**: `arr.push(x)`, `arr.splice(...)`, index assignment → observed.
- **Whole replacement**: replacing a subtree is fine and sometimes clearer:
  ```js
  appData.profile = { name: "Ada", role: "admin" }; // replace instead of mutating many fields
  ```

Guidelines
- Prefer **coarse, intentional updates** over many tiny mutations.
- Keep mutable state **close to where it is rendered**.
- If a value should not trigger updates, keep it out of the reactive data graph (e.g., store it in a closure or a non-reactive field you don’t bind).

## 6) Ownership & persistence

- **Per-host ownership**: Data initialized on a Nablla host belongs to that host. Treat it as the single source of truth for that component’s UI.
- **Sharing later**: Cross-component or cross-app sharing is possible, but this chapter does not cover it (see Scopes & Worlds). Start local; promote to shared only when necessary.
- **Persistence**: For saving/loading remote data, use higher-level directives/utilities (introduced elsewhere). Keep the **in-memory shape** simple so serialization remains trivial.

## 7) Error-prone cases (and how to avoid them)

- **Huge payloads in attributes**: Large JSON inside `data="..."` is hard to read and escape. Move it to script or load it remotely.
- **Circular structures**: JSON cannot represent cycles. Break the cycle or serialize to IDs.
- **Functions in data**: Do not embed functions in inline JSON. If needed, set handlers via attributes/events (Chapter 4) or compose them in script, not inside the data graph.
- **Non-JSON instances**: Convert `Map/Set/Date/BigInt` to plain equivalents (arrays, strings, numbers) before binding. Recreate rich types in script if required.
- **Over-nesting**: Prefer shallow structures. If you frequently write `a.b.c.d`, consider flattening or grouping into smaller sub-objects.

## 8) Minimal patterns (tiny examples)

Inline, small, serializable:
```html
<my-nablla data='{"count":0,"filters":{"q":""}}'></my-nablla>
```

Script, large or computed:
```html
const el = document.querySelector("my-nablla");
const initial = {
  title: "Catalog",
  items: Array.from({length: 1000}, (_,i)=>({ id:i+1, name:`Item ${i+1}` })),
  generatedAt: new Date().toISOString()
};
el.setAttribute("data", JSON.stringify(initial));
```

Replacing a section cleanly:
```js
// Instead of mutating many fields one-by-one:
profile = { name: "Lin", role: "editor", lang: "en" };
```

## 9) Summary (rules of thumb)

- Use **plain, JSON-like data**; keep it small and close to the UI that needs it.
- Initialize via **inline `data='...'`** for small cases; use **script** for large/typed/computed cases.
- Prefer **coarse, intentional updates** (sometimes whole-object replacement).
- Avoid cycles, functions, and non-serializable instances in inline data.
- Start local; introduce sharing only when the design clearly needs it.

---

Next: **Binding & Events** ? how `:prop`, `@event`, and `%%` connect data to the DOM and user actions.
