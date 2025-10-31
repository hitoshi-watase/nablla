# Grammar

_What you will find:_ a formal description of directive value shapes and handler syntax as implemented in Nablla v1.06. This page is normative, concise, and tutorial-free.

## 0. Legend (tokens and notation)

- `Expr` - a JavaScript **expression** evaluated by the runtime (statements are out of scope).
- `Ident` - a JavaScript identifier (ASCII suggestion; Unicode identifiers follow JS rules).
- `String` - an attribute string literal shown as `"..."` here (actual HTML quoting rules apply).
- `LValue` - a writable property reference inside host data:  
  `LValue ::= Ident { ('.' Ident) | ('[' Expr ']') }`
- `Mods` - event modifier list: zero or more `.`-prefixed modifier names.
- Braces `{ ... }` mean “repeat zero or more times”. Brackets `[ ... ]` mean “optional”. Literal commas are required where shown.
- All directive names are **lowercase** and **case-sensitive**.
- Every `*name` directive has an **equivalent** `n-name` form (not repeated below).

## 1. Common rules

- Attribute values are parsed as strings and then interpreted per rule below.
- Whitespace around separators is permitted.
- Truthiness and comparison use JavaScript semantics unless noted (e.g., `*switch` cases use `===`).
- Value-less directives are written without `="..."` and are equivalent to the presence of the flag.

## 2. Control flow

```
IfChain     ::= IfHead { ElseIf } [ Else ]
IfHead      ::= '*if="' Expr '"'
ElseIf      ::= element-with '*elseif="' Expr '"'
Else        ::= element-with '*else'

SwitchBlock ::= '*switch="' Expr '"' ... { Case | CaseBreak } [ Default ]
Case        ::= element-with '*case="' Expr '"'
CaseBreak   ::= element-with '*case.break="' Expr '"'
Default     ::= element-with '*default'
```

- Matching in `SwitchBlock` uses strict equality (`===`).

## 3. Iteration

```
Each        ::= '*each="' Expr ' as ' Ident [ ',' Ident ] '"'
For         ::= '*for="' Expr '"'
```

- In `Each`, `Ident` binds `item` and optional `index/key`.

## 4. Scope and variables

```
Let         ::= '*let="' AssignList '"'
Global      ::= '*global="' AssignList '"'
Literal     ::= '*literal="' Expr '"'
Rem         ::= '*rem="' String '"'

AssignList  ::= Assign { ',' Assign }
Assign      ::= LHS '=' Expr
LHS         ::= LValue
```

- During `*let` evaluation, `$parent` is injected (see expressions reference).

## 5. Input and staging

```
Input       ::= '*input="' LValue '"'
Lazy        ::= '*lazy'
Eager       ::= '*eager'
Stage       ::= '*stage'
Apply       ::= '*apply'
Restore     ::= '*restore'
Save        ::= '*save'
Load        ::= '*load'
```

- `*input` requires an `LValue` (pure expressions are not valid here).

## 6. Output

```
Print       ::= '*print="' Expr '"'
Compose     ::= '*compose="' Expr '"'
TextCont    ::= '*textContent="' Expr '"'
InnerHTML   ::= '*innerHTML="' Expr '"'
```

## 7. Attributes and shorthands

```
ClassBind   ::= ':class="' Expr '"'   | 'n-class="' Expr '"'
StyleBind   ::= ':style="' Expr '"'   | 'n-style="' Expr '"'
```

## 8. Communication (HTTP/API)

```
FetchEmpty  ::= '*fetch'
FetchExpr   ::= '*fetch="' Expr '"'
Post        ::= '*post="' Expr '"'
Api         ::= '*api="' Expr '"'
Into        ::= '*into="' Ident '"'
```

- `*into` accepts **identifier only** (dynamic names via expressions are not supported).

## 9. Files

```
Upload      ::= '*upload="' Expr '"'
Download    ::= '*download="' Expr '"'
```

## 10. WebSocket

```
WebSocket   ::= '*websocket="' Expr '"'
WsSend      ::= '*ws-send="' Expr '"'
```

- `Expr` resolves to a URL string or an implementation-defined config object.

## 11. Events and modifiers

```
Handler     ::= '@' EvName Mods? '="' Expr '"'
EvName      ::= Name
Name        ::= ASCII letter followed by { ASCII letter | digit | '-' | ':' }
Mods        ::= { '.' Mod }
Mod         ::= 'prevent' | 'stop' | 'once' | 'capture' | 'passive' | 'update' | 'noupdate'
```

- `EvName` must not contain `.` (dot) - dots are reserved for modifiers.
- Modifiers are **order-independent**; duplicates are ignored.
- `update` and `noupdate` are **mutually exclusive** (do not combine on the same handler).

## 12. Strict mode

```
Strict      ::= '*strict' | '*strict="' IdentList '"'
IdentList   ::= Ident { ',' Ident }
```

- Arguments must be **identifiers only** (no property access).

## 13. Shorthand equivalence

- Every directive appears in two equivalent spellings: `*name` and `n-name`.  
  Example: `*if="Expr"` ⇔ `n-if="Expr"`.

## 14. Non-goals of this grammar

- It does **not** describe expression semantics, filters, scheduler behavior, or event payloads.  
  See: [Expressions](./expressions.md), [Filters](./filters.md), [Lifecycle](./lifecycle.md), and [Events](./events.md).

---
Back to index: [`README.md`](./README.md)
