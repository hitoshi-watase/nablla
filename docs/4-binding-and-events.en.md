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

## Binding & Events - *input

Displaying data is only half of binding.  
A true binding model must also allow user input to update the underlying data.  
Nablla provides this reverse flow through the `*input` directive.

Unlike event handlers that must be written in JavaScript, `*input` links the value of an input element directly to a data property.  
When the user edits the field, the data is immediately updated, and any `*print` or related bindings depending on it are refreshed automatically.

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

This defines Nablla’s two-way binding:
- `*print` handles **data → view** updates.  
- `*input` handles **view → data** updates.

The two directions coexist without conflict because Nablla tracks both sides through the same reactive data object.

| Direction | Directive | Flow |
|------------|------------|------|
| Data to View | *print | One-way output |
| View to Data | *input | One-way input |
| Combined | *print + *input | Two-way binding |

This two-way link is the simplest form of interactivity in Nablla.  
The next section extends this idea to more complex user actions through events.

