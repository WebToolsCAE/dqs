# **DQS – DO Query String**

**DQS (DO Query String)** is a compact procedural data structure for generating and modifying canvas elements. It provides an expressive format for automated map manipulation, supporting creation, editing, deletion, layout, and styling. It's used by [Pathmind](https://pathmind.app) to generate and import map files.

## **Syntax Overview**

Each command begins with the `DO:` prefix and uses `/` to separate specs.

### General Structure

```
DO:<command>/<spec1>/<spec2>/...
```

Slash usage:
`/` 1-slash: used to start a spec or to close command
`//` 2-slash: used to make a simple connection between elements (only allowed between `DO:create` commands)
`///` 3-slash: used to target an edit command onto a create command from dqs, use after a create and follow up with an edit (mainly used in overwriting link display text)

## **Supported Commands**

### `create`

Creates a new thought.

**Required:**

* `/type:` – must be one of:

  * `text.thought`
  * `note.thought`
  * `attachment.thought`
  * `table.thought`
    
* `/cont:` – content of the element

**Optional:**

* `/loc:` – absolute position in format `x<INT>y<INT>`
* `/style:` – one or more styling specs
* `/is:` – assigns properties to an element

**Example:**

```
DO:create/type:text.thought/cont:"Hello+World"/loc:x500y300/
```

### `edit`

Edits an existing element by its **ID**.

**Important:** The ID has to originate from an existing element and must be placed with no additional text in a spec.
Example: `DO:edit/12345678912345/...`

**Optional specs:**

* `/cont:` – new content (`null` to clear)
* `/loc:` – new location (`null` to clear)
* `/style:` – style updates (`null` to clear)
* `/is:` – reassigns properties to an element

**Optional command extension:**

* `/DO:extend/ID/DO:create...` - edits an existing element to connect to a new element, push ID behind the `extend` command like this: `DO:edit/DO:extend/ID/DO:create..` 

### `delete` 

Deletes an existing element by ID.

```
DO:delete/12345678912345/
```

### `clearAll`

Clears all elements on the current plain.

```
DO:clearAll/
```

### `extend`

Extends from a specified thought with a visible connection by index from order of elements created in the current dq string by creating a new element.

**Ways to use:**

**index-create**
* `/<index>` – index of the element (created in range of elements in dq string)
* `/DO:create...` – new element as child of connection, following normal `create` command rules

Example:
```
DO:extend/1/DO:create....
```
**index-index**

Connect an indexed create with an indexed create, most efficient connection tactic usually referenced at the bottom of the file.

Example:
```
DO:extend/1/2/
```
*Connects index 1 with index 2*

## **Specifications**

### `/cont:`

Defines element content in quotes. Uses encoding rules:

* `+` - space
* `++` - line break
* `~` - can be used as a separator between space or line break inputs
  for example:
  `.../cont:"Helo+~++~+World!"/...`
* `null` - clears content (do not wrap in quotes)
* `image data` - this only applies for attachment thoughts, to embed images you just put in base64 data (with correct data:image/png;base64 syntax) inside quotes or you result to just embeding the link to an image Pathmind's proxy will fetch it and provide it on the map.

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
* `background` – canvas-level background control
* `innerhtml` – injects html styling onto an element
* `size` - sets the size of an image

#### Substyles:

* Font size: `12px` to `32px` (12,14,16,18,20,24,28,32)
* Font style: `bold`, `italic`, `underlined`
* Color: hex values like `#FF0000`

Theese substyles have to be placed after the target definition and after a colon.
For example:
`DO:create/type:text.thought/cont:"Hello+World!"/style:colorTag:#FFFFFF/style:allText:bold/style:"World!":24px/`

#### Background Styles (canvas-level)

* `/style:background:hide` – hides background
* `/style:background:show` – shows background

### Inner html:

* `/style:innerhtml:"html"` – include the html styling inside quotes

### Size:

* `/size:<WIDTH>x<HEIGHT>` - scale the included image to a desired size

Only allowed for create attachment thought commands, for example:

```
DO:create/type:attachment.thought/cont:"[base64]"/size:500x500/
```

### `/is:`

Used for assigning properties to an element.

Options:

* `is:isStart` – marks the element as a starting point
* `is:isEnd` – marks the element as an endpoint

**Example:**

```
/is:isStart/
```

## Defining IDs

* ID must be valid and match an element on the map
* Correct usage example where `12345678912345` is the ID

```
DO:edit/12345678912345/cont:"Updated+Text"/
```
