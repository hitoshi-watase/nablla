# Data Fundamentals

> Status: draft  
> Applies to: Nablla core, directive engine

## 3) From `*let` to `data` ? the smallest ladder

#### Guidance ? Prefer `data` over `*let`

`*let` is for special cases: tiny demos, temporary locals, or loop/branch-local values.  
For everyday code, define values in `data` and read them with directives (`*text`, `:prop`, etc.). Persisted or shared state should live in `data`.

### Step 1: Recap (`*let` makes a value and shows it)

```html
<na-blla>
  <p *let="message = 'Hello Nablla!'" *text="message"></p>
</na-blla>

  <p>Hello Nablla!</p>
^ rendered output (omit `<na-blla>`)

```

### Step 2: Make it stable (move the same value into `data`)

```html
<na-blla data='{"message":"Hello Nablla!"}'>
  <p *text="message"></p>
</na-blla>

  <p>Hello Nablla!</p>
^ rendered output (omit `<na-blla>`)
```

### Step 3: Another primitive (number)

```html
<na-blla data='{"count":0}'>
  <p *text="count"></p>
</na-blla>
```

<p>0 </p>
^ Rendered output (omit `<na-blla>`)


