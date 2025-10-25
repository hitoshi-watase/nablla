# Binding & Events (basic)
_Last updated: 2025-10-22_

> This chapter explains how Nablla connects data and the DOM through basic bindings and event attributes.  

Displaying data on screen is the first and most essential act in any framework.  
In many libraries, even this simple act depends on an intermediate template layer or a separate compilation step.  
Nablla removes that layer and lets HTML attributes themselves declare how data should appear.

Nablla treats the browser’s DOM as the template itself - no extra template grammar, no detour.

```html
<na-blla data='{"message":"Hello Nablla!"}'>
  <p *print="message"></p>
</na-blla>

<p>Hello Nablla!</p>
^ Rendered output (omit <na-blla>)
```

The attribute `*print="message"` injects the value of `message` directly into the element’s text.  
When the underlying data changes, Nablla automatically re-evaluates all attributes linked to that data.  
This continuous connection between the data object and the DOM element defines Nablla’s binding model.

Expressions are also supported:

```html
<na-blla data='{"a":2,"b":3}'>
  <p *print="a + b"></p>
</na-blla>

<p>5</p>
^ Rendered output (omit <na-blla>)
```

Unlike `%item%`, which expands text inline, `*print` binds the **whole element** as the display node.

| Feature | %item% | *print |
|----------|---------|--------|
| Scope | Text node | Element |
| Timing | During text expansion | During DOM build |
| Use case | Inline substitution | Standalone output |
| Expression | Supported | Supported |

`*print` establishes the one-way flow from data to view.  
The next directive, `*input`, introduces the opposite path - from view back to data.

---

### Shared Flow - input

Displaying data is only half of interaction.
To stay consistent, user input must update the same data that drives the view.
Nablla handles this shared flow through the *input directive.

Unlike event handlers that must be written in JavaScript, `*input` links the value of an input element directly to a data property.  
When the user edits the field, the data updates instantly, and every expression that refers to it is re-evaluated in the same scope.

```html
<na-blla data='{"message":"Hello"}'>
  <input *input="message">
  <p *print="message"></p>
</na-blla>

<input value="Hello">
<p>Hello</p>
^ Rendered output (omit <na-blla>)
```

Typing into the `<input>` updates `message`, which then re-renders the `<p>` element.  
No explicit listener, no manual update, and no intermediate framework state are required.

This describes how Nablla lets data and view share the same state, instead of binding one to another.
- `*print` handles **data → view** updates.  
- `*input` handles **view → data** updates.

The two directions coexist without conflict because Nablla tracks both sides through the same reactive data object.

| Direction | Directive | Flow |
|------------|------------|------|
| Data to View | *print | Read from data |
| View to Data | *input | Write to data |
| Shared Scope | *print + *input | Read and write the same data |

Both *print and *input refer to the same scoped data, so changes from either side are reflected naturally.
There is no direct link between elements - only a single source of truth shared within the scope.

The next section extends this idea to more complex user actions through events.

---

## Events

User interaction in Nablla follows the same principle as data sharing:  
HTML attributes remain the single source of truth.  
Instead of writing JavaScript listeners, event behavior is declared directly within the markup and evaluated in the same scoped context.

### Basic Example

In Nablla, any event attribute beginning with `@` such as `@click`, `@change`, or `@input` runs its expression inside the Nablla scope.  
This allows the code to access both `el` (the current element) and `$event` (the DOM event) without touching the global context.

```html
<na-blla data='{"count":0}'>
  <button @click="count++">+1</button> 
  <p *print="count"></p>
</na-blla>

<button>+1</button>
<p>0 -> 1 -> 2 ...</p>
^ Rendered output (omit <na-blla>)
```

When the user clicks the button, the expression runs within the same scoped data context.  
The variable `count` updates instantly, and every expression that refers to it is re-evaluated in the same scope.  
No external script or handler is needed - the attribute itself defines both the action and its context.

### Event Propagation and Scope

Each Nablla element maintains its own evaluation scope.  
When an event occurs, its expression is executed within that scope first, not in the global environment.  
This means a variable defined inside one Nablla world never leaks into another.

```html
<na-blla data='{"count":0}'>
  <button @click="count++">A +1</button>
  <p *print="count"></p>
</na-blla>

<na-blla data='{"count":100}'>
  <button @click="count++">B +1</button>
  <p *print="count"></p>
</na-blla>

<p>Each counter moves independently.</p>
^ Rendered output (omit <na-blla>)
```

Even though both buttons modify a variable named `count`,  
each belongs to a different Nablla instance and therefore a different scope.  
No cross-contamination occurs, because each world maintains its own `data` object and re-evaluation context.

Internally, Nablla evaluates event directives through  
`Function("scope", "el", "$event", "with(scope){ ... }")`,  
giving every handler access to its data, element, and event object.

### Element and Event Access

```html
<na-blla data='{"flag":false, "label":"Make it true."}'>
  <button @click="flag = true; label='Clicked!'" *print="label"></button>
  <p *print="flag"></p>
</na-blla>

<button>Clicked!</button>
<p>true</p>
^ Rendered output (omit <na-blla>)
```

Use $event

```html
<na-blla>
  <p @mouseover="console.log($event.currentTarget)">currentTarget in console log.</p>
</na-blla>

<p></p>
^ Rendered output (omit <na-blla>)
> [ on mouseover, currentTaget in console. ]
```
The $event object behaves as the standard DOM event,
so properties like type, target, and currentTarget are available

mods

```html
<na-blla>
  <p @mouseover.once="console.log($event.currentTarget)">currentTarget in console log once.</p>
</na-blla>

<p></p>
^ Rendered output (omit <na-blla>)
> [ on mouseover, currentTaget in console once. ]
```
The .once modifier runs the handler only the first time the event occurs.  

In Nablla, event expressions should update data, not the DOM itself.
Here, both the button label and the paragraph reflect the same state through *print.
When the user clicks, flag and label change, and the view updates automatically - no manual DOM manipulation is required.

| Symbol | Meaning |
|---------|----------|
| `el` | The current element handling the event |
| `$event` | The native DOM event object |
| `scope` | The current Nablla data context |

Nablla does not override native propagation.  
Events bubble and capture as usual, but their attached expressions are evaluated only within their own scope.  
This keeps user actions local, predictable, and consistent with the shared-state model established earlier.

In summary, Nablla treats events as part of the same shared flow:  
data <-> view <-> user action, all described directly in HTML.

---

### Summary

Nablla treats events as a natural part of the same shared flow that connects data and view.  
There are no handlers, bindings, or external callbacks - only attributes that describe how data behaves when users interact with it.

Event expressions run inside the same scoped data context,  
so updates stay local and predictable, even when multiple Nablla worlds coexist on a page.  
Each world re-evaluates only its own expressions, ensuring isolation and consistency.

| Concept | Description |
|----------|-------------|
| Evaluation | Event expressions are executed in the same Nablla scope |
| Update | Data changes trigger re-evaluation of related expressions |
| Isolation | Each world manages its own data and event flow |
| Result | User actions and DOM updates remain automatically synchronized |

Events are declared with @event attributes in HTML.  
Each event expression is evaluated in the Nablla scope of the element.  
On user action: data updates -> expressions that reference it re-evaluate -> the DOM updates via *print.  
Do not write to the DOM in handlers. Change data and let *print render.  
$event is available. el is not injected.  
Worlds are isolated. State and evaluation do not cross worlds.  
