# DQS – DO Query String Specification

**DQS (DO Query String)** is a compact procedural scripting language for generating and modifying structured thought maps. It provides a minimal yet expressive format for automated map manipulation, supporting creation, editing, deletion, layout, and styling.

## Syntax Overview

Each command begins with the `DO:` prefix and uses `/` or `//` to separate segments.

### General Structure

```
DO:<command>/<spec1>/<spec2>/...
```

Double slashes `//` are allowed between commands when chaining.

---

## Supported Commands

### `create`

Creates a new thought.

**Required:**

* `/type:` – must be one of:

  * `text.thought`
  * `note.thought`
  * `function.thought`
* `/cont:` – content of the thought

**Optional:**

* `/loc:` – absolute position in format `x<INT>y<INT>`
* `/style:` – one or more styling specs
* `/is:` – marks the thought as a start or end point

**Example:**

```
DO:create/type:text.thought/cont:"Hello+World"/loc:x500y300/
```

---

### `edit`

Edits an existing thought by its **ID**.

**Important:** The ID must be a 14–19 digit number and placed directly in the path without a label.
Example: `DO:edit/12345678912345/...`

**Optional specs:**

* `/cont:` – new content (`"null"` to clear)
* `/loc:` – new location
* `/style:` – style updates
* `/is:` – reassigns start or end markers
* `/extend:` – allows creation of new thought(s) directly from this one

---

### `delete`

Deletes an existing thought by ID.

```
DO:delete/12345678912345/
```

---

### `clearAll`

Clears all thoughts on the current map.

```
DO:clearAll/
```

---

### `extend`

Extends from a specified thought index by creating a new thought.

**Required:**

* `/index:` – index of the source thought (0-based)
* `/create...` – a `create` segment, following normal rules

---

## Specifications

### `/cont:`

Defines thought content. Uses encoding rules:

* `+` → space
* `++` → line break
* `"null"` → clears content

**Examples:**

* `cont:"Title+Here"` → `Title Here`
* `cont:"Line+One++Line+Two"` → multiline text
* `cont:"null"` → empty content

---

### `/loc:`

Sets location of a thought on the canvas.

Format: `x<INT>y<INT>`
Example: `loc:x900y400`

Use `loc:null` to use default placement logic.

---

### `/style:`

Applies styling to thought content or elements.

Format: `/style:<target>:<style>`
Supported targets:

* `allText` – overall style
* `"snippet"` – styled substring (include quotes)
* `colorTag` – for tag visuals
* `background` – map-level background control

#### Substyles:

* Font size: `12px` to `32px`
* Font style: `bold`, `italic`, `underlined`
* Color: hex values like `#FF0000`

#### Background Styles (map-level)

* `/style:background:hide` – hides background
* `/style:background:show` – shows background

---

### `/is:`

Used for marking special roles on a thought.

Values:

* `is:isStart` – marks as start point for executables
* `is:isEnd` – marks as endpoint

**Example:**

```
/is:isStart/
```

---

## ID Conventions

* Thought IDs must be 14 to 19 digits.
* In `edit`, `delete`, etc., only the ID is used, without a key:

```
DO:edit/12345678912345/cont:"Updated+Text"/
```

---

## Notes

* All commands must begin with `DO:`
* Paths are strictly ordered
* Commands can be chained using `//`
* Function-type thoughts require valid URLs in `/cont:`
* Styling is extensible for future enhancements (e.g., grid backgrounds, background images)

---

## Future Extensions (Planned)

* `/style:background:image("url")`
* `/style:background:grid(true)`
* `/style:background:color("#000000")`
* `/style:border:...` and `/style:font:...`

