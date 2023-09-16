---
title: "Scribunto for Stars"
subtitle: "A crash course on converting and designing templates with Lua"
author:
- FishTank
description: A workshop series designed for Fandom Stars.
lang: en-US
Google-Slides: https://docs.google.com/presentation/d/1jtTVOaZPYCQSlrB734stAuupVGyCfO72P4zXITaplJA/edit?usp=sharing
...

# Scribunto for Stars
A workshop series designed for Fandom Stars

## A quick introduction to Lua
Scribunto is MediaWiki's platform for the Lua language. When we refer to "Lua", we usually mean the special dialect of the Lua language that is an alternative to wikitext for making templates.

## Modules and functions (and tables — oh my!)
Module
  ~ A wiki page, in the `Module:` namespace.

Function
  ~ An action stored in a Module page. Functions are stored as "local", "global", or values in a table. Local functions can only be used by other functions in the same Module, so they're perfect for "helper" functions rather than nesting templates. Functions that are values in the Module's "returned" table can be invoked by a template.
  
Table (in a Lua sense)
  ~ A structure that stores data in a list (array), a set of values referenced by a "key" (hash), or a combination of these.


### Invoke is the magic word

## What's a type? (for non-programmers)

## What's a library? (also: further reading)


------------------
# Session 1: Converting templates
One of the strengths of Lua over wikitext is that logical work can be done separately from the output, leading to code that is easier to interpret than nested brackets and awkward parser functions.


## Many happy `return`s
Because templates can produce code without any input, let's talk about output.

```lua!
function facts()
    return 'Lua is more complicated than wikitext, but is faster and less wasteful.'
end
```

## Template arguments
:::info
There is a long way to do it, but the [Arguments](https://dev.fandom.com/wiki/Global_Lua_Modules/Arguments) Global Lua Module is popular and conveniently makes processing much easier.
:::

## Replacing `{{#switch:}}` with table lookups
Switches are 
```mediawiki!=
{{#switch:{{{1|}}}
| case 1 = Foo
| case 2 = Bar
| case 3
| case 4 = FooBar
| #default: {{{1|}}}
}}
```
can be converted to
```lua!=
local example = {}

function example.fooBar( frame )
    local fooBarText = frame.args[1]
    local default = ""
    local fooBarData = {
        ["case 1"] = "Foo",
        ["case 2"] = "Bar",
        ["case 3"] = "FooBar",
        ["case 4"] = "FooBar",
        --["default"] = ""
    }
    return fooBarData[fooBarText] or default or ""
end

return example
```
## Testing for truth

:::info
People either love or hate the ternary operator, but it potentially eliminates a lot of steps. We'll talk about it more later, but `cow = moos and true or false` can work very well.
:::

## Concatenation nation

:::info
Instead of adding more and more to a string, adding to a table and using the `table.concat` functions is faster and more flexible. There's even a `mw.text.listToText( list, separator, conjunction )` that makes "1, 2, 3, 4 and 5" very convenient.
:::
## Of `pairs` and `ipairs`

------------------
# Session 2: Designing from scratch

## The `mw.html` library
Part of what makes Lua faster is when it produces clean HTML code directly, skipping the processing step from wikitext to HTML. 

This is also a good way to introduce the "object method" style of functions. With this method, HTML is assembled as a complex table with functions like added elements and attributes strung on it like beads.

## The `string` library and patterns

## Compact code with ternary operators

## The many objects of `mw.title` 

## Loading lots of data

------------------
# Bonus: Docbunto