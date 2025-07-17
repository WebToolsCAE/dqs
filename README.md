# **DQS – DO Query String**

**DQS (DO Query String)** is a compact procedural scripting language for generating and modifying structured thought maps. It provides a minimal yet expressive format for automated map manipulation, supporting creation, editing, deletion, layout, and styling.

## **Syntax Overview**

Each command begins with the `DO:` prefix and uses `/` to separate specs.

### General Structure

```
DO:<command>/<spec1>/<spec2>/...
```

Double slashes `//` are allowed between create commands for connecting the elements with a visual connection.


## **Supported Commands**

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

### `edit`

Edits an existing element by its **ID**.

**Important:** The ID must be a 14–19 digit number and placed directly in the path without a label.
Example: `DO:edit/12345678912345/...`

**Optional specs:**

* `/cont:` – new content (`null` to clear)
* `/loc:` – new location (`null` to clear)
* `/style:` – style updates (`null` to clear)
* `/is:` – reassigns start or end markers

**Optional command extension:**

* `/DO:extend/ID/DO:create...` - edits an existing element to connect to a new element, push ID behind the `extend` command like this: `DO:edit/DO:extend/ID/DO:create..` 

### `delete` 

Deletes an existing element by ID.

```
DO:delete/12345678912345/
```

### `clearAll`

Clears all thoughts on the current map.

```
DO:clearAll/
```

### `extend`

Extends from a specified thought with a visible connection by index from order of elements created in the current dq string by creating a new element.

**Required:**

* `/index` – index of the element (created in range of elements in dq string)
* `/DO:create...` – new element as child of connection, following normal `create` command rules

## **Specifications**

### `/cont:`

Defines element content in quotes. Uses encoding rules:

* `+` - space
* `++` - line break
* `null` - clears content (do not wrap in quotes)

**Examples:**

* `cont:"Title+Here"` 

  Title Here

* `cont:"Line+One++Line+Two"` 

  Line One

  Line Two

* `cont:null`

  (no text)

### `/loc:`

Sets location of a thought on the canvas.

Format: `x<INT>y<INT>`
Example: `loc:x900y400`

Use `loc:null` to use default placement logic.

### `/style:`

Applies styling to element content or background.

Format: `/style:<target>:<style>`
Supported targets:

* `allText` – all text in element
* `"snippet"` – styled substring, a part of element's text (include quotes)
* `colorTag` – background coloring
* `background` – map-level background control

#### Substyles:

* Font size: `12px` to `32px`
* Font style: `bold`, `italic`, `underlined`
* Color: hex values like `#FF0000`

#### Background Styles (map-level)

* `/style:background:hide` – hides background
* `/style:background:show` – shows background

### `/is:`

Used for marking special roles on a thought.

Values:

* `is:isStart` – marks as start point for executables
* `is:isEnd` – marks as endpoint

**Example:**

```
/is:isStart/
```

## Defining IDs

* ID must be valid and match element on map
* Correct usage example where `12345678912345` is the ID

```
DO:edit/12345678912345/cont:"Updated+Text"/
```
