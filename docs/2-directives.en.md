# Directives Overview  
Last updated: 2025-10-15  

## 1) What Are Directives  
Directives are Nablla’s attribute-based mini-commands.  
They extend plain HTML with logic such as loops, conditions, and data binding ? yet remain fully valid markup.  
Every directive starts with an asterisk (`*`) and acts on its host element.  

Example:  
```html
<p *let="message='Hello Nablla!'">%message%</p>
<!-- Result → Hello Nablla! -->
```

Here, `*let` defines a local variable `message`, and `%message%` expands its value directly into the DOM.  
Interpolations like `%...%` work in both **text** and **attribute** values.

---

## 2) Core Directives  

### **`*let` ? Define a local variable**  
Defines a scoped variable for the current element and its children.  
Works in text nodes **and** attributes.  

```html
<section *let="color='skyblue'">
  <p style="color:%color%">This text is %color%.</p>
</section>
<!-- Result → A blue-colored text “This text is skyblue.” -->
```

You can also use multiple variables:
```html
<div *let="x=2, y=3">Sum: %x + y%</div>
<!-- Result → Sum: 5 -->
```

---

### **`*text` ? Bind text content**  
Replaces the element’s text with the variable value.  
Equivalent to writing `%var%` inside, but easier to read when nesting.

```html
<h1 *text="title"></h1>
<!-- Result → Displays the current value of “title” -->
```

If you prefer inline style:
```html
<h1>%title%</h1>
```
Both are equivalent.

---

### **`*model` ? Two-way binding**  
Connects form elements (`input`, `textarea`, `select`) to variables.  
Any user input updates the variable, and data updates refresh the UI.

```html
<na-blla>
  <input *model="user" placeholder="Type your name" />
  <p>Hello, %user%!</p>
</na-blla>
<!-- Result → Typing updates “Hello, (your name)!” instantly -->
```

---

## 3) Conditional Directives  

### **`*if` ? Conditional rendering**  
Displays the element only when the expression is truthy.  
```html
<p *if="loggedIn">Welcome back!</p>
<!-- Result → Visible only when loggedIn === true -->
```

If `loggedIn` becomes false, the element is removed from the DOM but can reappear later.

---

### **`*switch` / `*case` ? Multi-branch rendering**  
Renders exactly one matching branch.  
```html
<section *switch="status">
  <p *case="'idle'">Waiting…</p>
  <p *case="'loading'">Loading...</p>
  <p *case="'done'">Done!</p>
</section>
<!-- Result → Shows only the paragraph matching current “status” -->
```

---

### **`*for` ? Loop with index**  
Provides both item and index, similar to a standard `for` loop.  
```html
<ol>
  <li *for="(user,i) in users">%i%. %user%</li>
</ol>
<!-- Result → 1. Alice / 2. Bob / 3. Carol ... -->
```

---

## 4) Iteration Directives  

### **`*each` ? Iterate through arrays**  
Repeats an element for each item in a list.  
```html
<ul *each="item in items">
  <li>%item%</li>
</ul>
<!-- Result → Generates a <li> for every item -->
```

---

## 5) Summary  

| Directive | Purpose | Example | Result |
|------------|----------|----------|--------|
| `*let` | Define local variable | `<p *let="x=5">%x%</p>` | `5` |
| `*text` | Bind text node | `<h1 *text="title"></h1>` | Displays title |
| `*model` | Two-way binding | `<input *model="name" />` | Updates `name` live |
| `*if` | Conditional display | `<p *if="ok">Yes</p>` | Only if `ok` |
| `*switch` / `*case` | Multi-branch | `<p *case="'on'">On</p>` | Depends on `switch` |
| `*for` | Loop with index | `<li *for="(x,i) in list">%i%: %x%</li>` | Indexed list |
| `*each` | Loop items | `<li *each="x in list">%x%</li>` | List items |

---

Next chapter: **Reactivity and Observe Modes** (`docs/3-reactivity.en.md`)  
