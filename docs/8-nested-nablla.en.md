# Nested Nablla  
_Last updated: 2025-10-22_

> This chapter explains how Nablla behaves when multiple `<na-blla>` elements are nested inside one another.  
> You will learn how each Nablla keeps its own data scope and how they interact.

---

## 1. Overview

Sometimes you may want to use more than one `<na-blla>` on the same page.  
For example, an outer Nablla might hold page-wide data, while an inner Nablla handles a smaller section such as a comment box or a sidebar.  

Nablla supports this naturally ? every `<na-blla>` works as an independent world with its own data and lifecycle.  
This means that updates in one Nablla do not interfere with others, even when they are nested.

## 2. Different update timing

Nested Nablla behaves differently from a single Nablla when you use input bindings.

Compare the following two cases.

---

### Case 1: Single Nablla

```html
<na-blla data='{"text":"update when confirmed"}'>
  <input *input="text">
  <p>%text%</p>
</na-blla>

<p>update when confirmed</p>
^ Rendered output (omit `<na-blla>`)
```

Here, the `<p>` element updates **only after the input is confirmed**.  
While typing, the value inside `<p>` does not change.

---

### Case 2: Nested Nablla

```html
<na-blla data='{"text":"update while typing"}'>
  <input *input="text">
  <na-blla>
    <p>%text%</p>
  </na-blla>
</na-blla>

<p>update while typing</p>
^ Rendered output (omit `<na-blla>`)
```

When a second `<na-blla>` is placed inside, the behavior changes:  
the inner Nablla shows the same value as you type, updating simultaneously with the input field.

---

The two cases look similar, but their timing differs:
- **Single Nablla:** waits until the input event completes  
- **Nested Nablla:** updates instantly with every keystroke

## 3. Independent scopes

Each `<na-blla>` works with its own data area.  
Even when nested, inner and outer Nablass do not automatically share values ?  
each one manages its own copy of data.

```html
<na-blla data='{"text":"We are the world."}'>
  <h2 *print="text"></h2>

  <na-blla data='{"text":"We are the children."}'>
    <p *print="text"></p>
  </na-blla>
</na-blla>

<h2>We are the world.</h2>
<p>We are the children.</p>
^ Rendered output (omit `<na-blla>`)
```

Here both Nablass use the same property name (`text`),  
but they belong to separate data areas.  
Changing the outer value will not affect the inner one, and vice versa.

This separation lets you build multiple self-contained parts on the same page  
without worrying about variable conflicts.

---

## 4. Data inheritance and separation

When Nablla elements are nested,  
the child Nablla can use the parent's data as long as it does not define its own.  
Once the child declares its own `data`,  
it becomes completely independent ? the parent's values are no longer visible.

---

### Case 1: Shared scope 

```html
<na-blla data='{"word":"floccinaucinihilipilification"}'>
  <p>The word meaning 'to regard something as worthless' is %word%. Repeat.</p>
  <na-blla>
    <p>The word is %word%.</p>
  </na-blla>
</na-blla>

<p>The word meaning 'to regard something as worthless' is floccinaucinihilipilification. Repeat.</p>
<p>The word is floccinaucinihilipilification.</p>
^ Rendered output (omit `<na-blla>`)
```

Here the inner Nablla defines no `data`,  
so it repeats the same word,  
echoing perfectly inside the same world.

---

### Case 2: Isolated scope

```html
<na-blla data='{"word":"floccinaucinihilipilification"}'>
  <p>The word meaning 'to regard something as worthless' is %word%. Repeat.</p>
  <na-blla data='{"food":"focaccia"}'>
    <p>The word is %word%... I want to eat a %food%.</p>
  </na-blla>
</na-blla>

<p>The word meaning 'to regard something as worthless' is floccinaucinihilipilification. Repeat.</p>
<p>The word is ... I want to eat a focaccia.</p>
^ Rendered output (omit `<na-blla>`)
```

When the inner Nablla defines its own `data`,  
it no longer refers to the outer `word`.  
Only the variables inside its own `data` remain visible,  
so `%word%` shows nothing while `%food%` appears normally.

---

Nablla's nested design keeps each element independent by default.  
Only when the child omits its own `data` does it rely on the parent's values.  
This separation makes nested components easy to reason about and prevents hidden dependencies.

---

## 5. Common mistakes

When working with nested Nablass, beginners sometimes assume that data automatically flows between them.  
In practice, each Nablla controls its own scope.

| Mistake | What happens | Correct understanding |
|----------|---------------|------------------------|
| Expecting automatic sharing | The inner Nablla can access parent data only when it has no `data` of its own. Beginners sometimes expect both to stay linked even after defining child `data`. | Data is inherited automatically only when the child defines no `data`. Once defined, it becomes a separate scope. |
| Expecting parent values after defining child `data` | After the child defines its own `data`, variables from the parent become unavailable. | A new `data` creates a new scope. Only the child's variables remain visible. |
| Trying to update parent data | Changes made inside the inner Nablla do not affect the outer one. | Every Nablla manages its own data independently. |

These behaviors are intentional.  
By keeping data local to each Nablla,  
the framework avoids accidental side effects and makes nested structures predictable.

---

## 6. let inside nested scopes

When `*let` is used inside a nested Nablla,  
it can **read** values created by the parent's `*let`,  
but it **cannot modify** them.  
Each Nablla still maintains its own local context.

```html
<na-blla *let="count=1">
  <p>Outer: %count%</p>
  <na-blla *let="count=count+1">
    <p>Inner: %count%</p>
  </na-blla>
</na-blla>

<p>Outer: 1</p>
<p>Inner: 2</p>
^ Rendered output (omit `<na-blla>`)
```

Here the inner Nablla reads the parent's `count`  
and creates a new one based on it.  
The parent's value remains unchanged.

In Nablla, `*let` works like a bridge that copies values at creation time,  
not a live link between scopes.  
The next chapter explains how to connect them more directly.

---

## 7. Summary

Nested Nablla shows how each **Nablla context** (whether `<na-blla>` or another host) forms its own scope.  
Key points:

- Each context manages its own `data` and lifecycle.  
- A child context inherits the parent's values **only when it defines no `data` of its own**.  
- Once the child defines `data`, the two contexts are **isolated**.  
- Updates inside the child do **not** affect the parent.  
- `*let` inside a child can **read** values introduced by the parent's `*let`, but **cannot modify** them.
