# Directives Overview  
Last updated: 2025-10-15  

## 1) What Are Directives  
Directives are Nablla’s attribute-based mini-commands.  
They extend plain HTML with logic such as looping, conditional display, or data binding ? yet remain fully valid markup.  
Every directive starts with an asterisk (`*`) and acts on its host element.  

Example:  
```html
<p *let="message='Hello Nablla!'">%message%</p>
```
Here, `*let` creates a local variable `message`, and `%message%` prints its value.  

---

## 2) Core Directives  

### **`*let` ? Define a local variable**  
Defines a scoped value for the current element and its children.  
```html
<section *let="color='skyblue'">
  <p style="color:%color%">This text is %color%.</p>
</section>
```  
Variables declared with `*let` are reactive ? when the source data changes, attributes update automatically.  

---

### **`*text` ? Bind text content**  
Replaces the element’s text with a variable value.  
```html
<h1 *text="title"></h1>
```
Equivalent to writing `%title%` inside the element, but explicit and easier to parse when nesting.  

---

### **`*model` ? Two-way binding**  
Links form elements (`input`, `textarea`, `select`) with data.  
Changes in either side propagate instantly.  
```html
<na-blla>
  <input *model="user" placeholder="Type your name" />
  <p>Hello, %user%!</p>
</na-blla>
```
This keeps `user` synchronized both ways ? from data → attribute → data.  

---

## 3) Conditional Directives  

### **`*if` ? Conditional rendering**  
Renders the element only when the condition is truthy.  
```html
<p *if="loggedIn">Welcome back!</p>
```
If `loggedIn` is `false`, Nablla detaches the element from the DOM.  

---

### **`*switch` / `*case` ? Multi-branch rendering**  
Controls exclusive blocks.  
```html
<section *switch="status">
  <p *case="'idle'">Waiting…</p>
  <p *case="'loading'">Loading...</p>
  <p *case="'done'">Done!</p>
</section>
```
Nablla renders only the case that matches `status`.  

---

## 4) Iteration Directives  

### **`*each` ? Iterate through arrays**  
Repeats an element for every item in a list.  
```html
<ul *each="item in items">
  <li>%item%</li>
</ul>
```
Nablla clones the `<li>` template for each `item`.  

---

### **`*for` ? Advanced iteration (with index)**  
Provides item and index, similar to a traditional `for` loop.  
```html
<ol *for="(user,i) in users">
  <li>%i%. %user%</li>
</ol>
```

---

## 5) Summary  
| Directive | Purpose | Example |
|------------|----------|----------|
| `*let` | Define a local variable | `<p *let="x=5">%x%</p>` |
| `*text` | Bind text node | `<h1 *text="title"></h1>` |
| `*model` | Two-way data binding | `<input *model="name" />` |
| `*if` | Conditional display | `<p *if="ok">Yes</p>` |
| `*switch` / `*case` | Multi-branch | `<div *switch="mode"><p *case="'on'"></p></div>` |
| `*each` | Simple loop | `<li *each="x in list">%x%</li>` |
| `*for` | Loop with index | `<li *for="(x,i) in list">%i%: %x%</li>` |

---

Next chapter: **Reactivity and Observe Modes** (`docs/3-reactivity.en.md`)  
