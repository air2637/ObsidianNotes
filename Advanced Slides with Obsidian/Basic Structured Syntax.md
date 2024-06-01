##### Slides level

 `<!-- slide template="[[tpl-con-slash]]" bg="orange" -->`

`<!-- .slide: data-auto-animate --> `

`<!-- .slide: style="background-color: coral;" --> `

`<!-- .slide: id="InternalLinks" -->`
##### Individual element level

`<!-- element style="background-color: green;" -->`

`<!-- element class="fragment" -->`

```md
<style>
	.with-border{
		border: 1px solid red;
	}
</style>

styled text <!-- element class="with-border" -->

```

##### Split components (portions) in a slide

Split to manipulate the layout in columns, rows:
- even
- gap
- left & right with proportion
- wrap - define how many columns in a row
- no-margin
##### Grid components (portions) in a slide

basic syntax `<grid drag="{width}, {height}" drop="{x-coordinate}, {y-coordinate}"` : the drag attribute defines the size of the component, the drop attribute defines the position of the component

Important:

- A positive x value indicates a position relative to the left edge of the slide.
- A negative x value indicates a position relative to the right edge of the slide.
- A positive y value indicates a position relative to the top edge of the slide.
- A negative y value indicates a position relative to the bottom edge of the slide.

##### Template

A template defines a unique layout, structure that can be applied to slides as a boilerplate code.

Define variable in template with `<% variableNameHere %>`

Basic syntax to define a template, by creating a file say 'tpl-footer'

```md
<% content %>

<grid drag="100 6" drop="bottom">
<% footer %>
</grid>
```

Use of the template

```md
<!-- slide template="[[tpl-footer]]" -->

# This header will be part of the content section defined in the template

Everything define outside a block will be placed in the content section.
Therefore every template has to contain at least a content variable.

To place a text into the footer section you have to create a block comment with the name of the variable you defined in the template.

::: footer
#### This header will be placed in the footer section of the template
:::

```

Make a variable in template optional with `?`

```md
<% content %>

<grid drag="100 6" drop="bottom">
<%? footer %>
</grid>

```

Make a template as the default:

```md
---
defaultTemplate: "[[tpl-footer]]"
---
```

