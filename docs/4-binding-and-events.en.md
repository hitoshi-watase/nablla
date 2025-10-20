## Binding & Events - *print

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

