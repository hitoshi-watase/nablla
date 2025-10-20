# Data Fundamentals

> Status: draft  
> Applies to: Nablla core, directive engine

## 3) From `*let` to `data` ? the smallest ladder

#### Guidance ? Prefer `data` over `*let`

In Chapter 2, we used `*let` to keep examples minimal and easy to follow.  
In everyday code, define values in `data` and read them with directives (`*pring`, `:prop`, etc.).  
Use *let only for short-lived locals (loops/branches).

### Step 1: Recap (`*let` makes a value and shows it)

```html
<na-blla>
  <p *let="message = 'Hello Nablla!'" *pring="message"></p>
</na-blla>

<p>Hello Nablla!</p>
^ rendered output (omit `<na-blla>`)

```

### Step 2: Make it stable (move the same value into `data`)

```html
<na-blla data='{"message":"Hello Nablla!"}'>
  <p *pring="message"></p>
</na-blla>

<p>Hello Nablla!</p>
^ rendered output (omit `<na-blla>`)
```

### Step 3: Another primitive (number)

```html
<na-blla data='{"count":0}'>
  <p *pring="count"></p>
</na-blla>

<p>0</p>
^ Rendered output (omit `<na-blla>`)
```

### Step 4: Quick variants (boolean and null)

```html
<na-blla data='{"flag":true}'>
  <p *pring="flag"></p>
</na-blla>

<p>true</p>
^ Rendered output (omit `<na-blla>`)
```

```html
<na-blla data='{"note":null}'>
  <p *pring="note"></p>
</na-blla>

<p></p>
^ Rendered output (omit `<na-blla>`)
```

### Step 5: A tiny object (two fields)

```html
<na-blla data='{"item":"Pencil","stock":12}'>
  <p *pring="item"></p>
  <p *pring="stock"></p>
</na-blla>

<p>Pencil</p>
<p>12</p>
^ Rendered output (omit `<na-blla>`)
```

### Step 6: A small array (iterate with `*for`)

```html
<na-blla data='{"items":["Pencil","Notebook","Eraser"]}'>
  <ul>
    <li *for="item of items">%item%</li>
  </ul>
</na-blla>

<ul>
  <li>Pencil</li>
  <li>Notebook</li>
  <li>Eraser</li>
</ul>
^ Rendered output (omit `<na-blla>`)
```

### Note: `data` is always text

The `data` attribute is always a text value. Nablla reads and parses it as JSON when the component starts.  
This is why all keys and strings must be quoted and why functions cannot be placed directly inside `data`.

## Summary

1. **Data stays simple** ? start with one value, then small objects or arrays.  
2. **Prefer `data` over `*let`** ? use `data` for anything persistent or shared.  
3. **Keep JSON valid and small** ? double quotes, flat shape, no functions.  
4. **Arrays loop naturally with `*for`** ? no helpers needed.
