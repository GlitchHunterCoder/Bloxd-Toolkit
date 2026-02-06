## 🔍 Bloxd Custom Games – Smart Search Engine

This search engine supports a **full query language** with
- boolean logic,
- field targeting,
- fuzzy search,
- regex, numeric comparisons,
- and even custom JavaScript filters.

---

## 📦 Indexed Fields

The following fields can be searched directly:

* `name`
* `description`
* `gameCategory`
* `schematicId`
* `gamePublished`
* `ccu`
* `favourites`
* `all` (special field – searches across *every* field)

> Field names are **auto-corrected** if mistyped.

---

## 🧠 Basic Search

### Fuzzy Text Search (Default)

Searches across all fields using substring matching.

```
parkour
zombie
```

Equivalent to:

```
all:parkour
```

---

## 🎯 Field-Specific Search

Target a specific field using `field:value`.

```
name:parkour
gameCategory:adventure
```

---

## ✨ Exact Match

Use quotes to match a value **exactly**.

```
name:"Parkour Master"
```

---

## 📚 Grouped Terms

Match **any** word in a group.

```
name:(parkour speedrun race)
```

Matches if *any* term appears.

---

## 🧩 Boolean Logic

### AND (explicit or implicit)

```
parkour ccu:{ x >= 50 }
```

```
parkour AND favourites:{ x > 10 }
```

---

### OR

```
gameCategory:parkour OR gameCategory:pvp
```

---

### NOT / Exclusion

```
parkour NOT tutorial
```

or

```
parkour -tutorial
```

---

## 🔢 Numeric Comparisons

Use `{}` blocks for numeric filters.

```
ccu:{ x > 100 }
favourites:{ x >= 25 }
```

Supported operators:

* `>`
* `>=`
* `<`
* `<=`

---

## 🧠 Custom JavaScript Filters (Advanced)

Any `{}` block that is **not** a simple comparison becomes a JavaScript function.

### Examples

```
ccu:{ x % 2 === 0 }
```

```
favourites:{ return x > 10 && x < 100 }
```

You can access:

* `x` → current field value
* `Search` → full item
* `Data` → entire dataset
* `engine` → search engine instance

---

## 🌍 Global Logic (`all:{ ... }`)

Run logic against **every field**.

```
all:{ x.includes("parkour") }
```

If *any field* returns true, the item matches.

---

## 🧬 Regex Search

Use JavaScript-style regex literals.

```
name:/parkour/i
description:/speed\s*run/i
```

Flags supported (`i`, `g`, etc).

---

## 🔍 Parentheses & Precedence

Group expressions explicitly.

```
(name:parkour OR name:racing) AND ccu:{ x >= 50 }
```

---

## ✨ Implicit AND

Multiple expressions next to each other automatically AND together.

```
parkour favourites:{ x > 10 }
```

---

## 🪄 Field Name Autocorrection

Misspelled fields are auto-corrected using Levenshtein distance.

```
favrites:{ x > 10 }
```

✔ Automatically corrected to:

```
favourites
```

---

## 🧠 Scoring & Fuzzy Matching

* Fuzzy matches use substring logic
* Custom functions can return numbers for scoring
* Highest score wins for relevance sorting

---

## 📊 Sorting Options

Results can be sorted by:

* Relevance
* CCU (ascending / descending)
* Favourites (ascending / descending)
* Name (A → Z)

---

## 🧪 Example Queries

```
name:"Parkour Pro" AND ccu:{ x >= 100 }
```

```
(gameCategory:parkour OR gameCategory:racing) favourites:{ x > 25 }
```

```
all:{ x.includes("hard") } NOT tutorial
```

```
description:/speed(run)?/i
```

---

## ⚠️ Notes

* Empty query returns **all results**
* Invalid filters safely fail (no crashes)
* JavaScript filters are sandboxed per item
