[//]: # "Note: this is [one way to do] a markdown comment"
[//]: # "Note: numbered lists will auto-generate the numbers in the output, even if the numbers are out of order. For maintainability, they're all 1, so the order can be changed without making it look chopped-up"
[//]: # "For further reading, see https://www.markdownguide.org/"

# Table of Contents
- [Table of Contents](#table-of-contents)
- [General](#general)
    * [Philosophy](#philosophy)
    * [Document Layout](#document-layout)
- [Languages](#languages)
    * [Universal](#universal)
        + [Indentation](#indentation)
            - [Tabs vs Spaces:](#tabs-vs-spaces)
            - [How Many Spaces?](#how-many-spaces)
        + [Naming](#naming)
    * [Object-Oriented](#object-oriented)
        + [Access Modifiers](#access-modifiers)
        + [Strings](#strings)
        + [Brackets/Braces](#bracketsbraces)
        + [Class Member/Variable Declaration/Initialization](#class-membervariable-declarationinitialization)
    * [TypeScript/JavaScript](#typescriptjavascript)
        + [General](#general-1)
        + [Semicolons](#semicolons)
        + [Quotes](#quotes)
        + [Variable Declarations](#variable-declarations)
        + [Functions](#functions)
        + [Type Specification](#type-specification)
    * [C#](#c)
    * [HTML](#html)
    * [CSS/SCSS/SASS](#cssscsssass)
    * [Angular](#angular)
        + [CSS](#css)
        + [TypeScript](#typescript)
        + [HTML](#html-1)
- [Editors](#editors)
    * [Visual Studio](#visual-studio)
    * [VS Code](#vs-code)
    * [SSMS](#ssms)
- [Git](#git)
- [Appendix](#appendix)

# General
## Philosophy
The purpose of this document is to promote code standards with four main targets:
- Readability (write code for other people to read)
- Consistency between languages and environments (develop muscle memory)
- Efficiency of writing (let the tools do the work)
- Maintainability of the product (less is more)

These standards may seem complicated on the surface, but collectively reflect an organized approach to writing code. Well-written code should be written more for *humans* to read, than for computers. As part of the values listed above, readability, modularity, and consistency are key aspects to this approach, to make it *easy* for other people (and yourself) to read and understand.

## Document Layout
Each rule/guideline in this document should have a (ideally objective) reason. Arbitrary decisions or personal preferences can be included, but should be based on something that can be argued to have tangible benefits.

    project organization
    front-end model generation

# Languages
## Universal
### Indentation
#### Tabs vs Spaces:
Historically, there has been some debate about whether indentation should consist of tab characters (\t) or space characters, but the overwhelmingly-majority opinion today is to use spaces. The primary reason why is consistency of display. In a monospace font, indentation will always be visually identical in any text editor if spaces are used. By contrast, tabs can be all over the place. 

#### How Many Spaces?
4-space indentation should be used almost universally, in every language. While there is some debate among programmers, 4-space is easier to read. When visually skimming a file, it's easier to follow the indentation levels and tell which block level you're in.

A potential exception is "system-generated" files. These can include project-related files maintained by an IDE, JSON files created by a CLI tool, bundled/minified output files, etc. While they can sometimes be configured to be 4-space, if you never/rarely need to look at or touch them, then there's not much reason to fight the tool.

### Naming
The naming process of variables, functions, methods, classes, files, and other coding-related objects can be generalized to two rules:
1. Name the thing based on what data it represents or what it does
1. Be as concise or verbose as you need to be in the wording, to make it immediately understandable for someone else what data it represents or what it does

Single-letter variable names are not self-descriptive. The single notable exception is `i`, as an index variable in an iterative context (i.e. for loops, single-level iterator/generator callbacks, etc.), but only when it's immediately obvious that that's what it is. In addition, `j`, `k`, and so on, can also be appropriate in nested for loops, though they should be rarely used in real-world code. Nested callbacks should be avoided, as discussed elsewhere, but if used, the index-variable names should instead be changed to match the data they represent (often the name of the collection followed by `Index`). However, even `i` is not always appropriate for those cases. For example, using `index` in a click-handler callback may be better than `i`, if the argument being passed isn't necessarily the consecutive zero-based number of the item in the list, like when a table is sorted, but you want the order they came in originally. In addition, in a click handler in JS file, for example, the iteration itself may be in the HTML, and so `index` (or `listItemIndex`, or something related to the type of item being clicked) may be more immediately self-explanatory than `i`.

For-each loops are different from for loops. If you're using an iterator (as in a for-each loop), you're getting an item from the collection, not indexing the collection directly. The variable should consequently be named according to the data it represents (for example, `for each car in cars`, or `for each selectedStatusId in selectedStatusIds`).

Along the same vein, there is rarely reason abbreviate words in names. Exceptions include obvious ones like `identifier` -> `id`, `number` -> `num`, `minimum`/`maximum` -> `min`/`max`, and often `index` -> `i`. Shortened versions of words that aren't in common vernacular should be avoided, like `control` -> `ctrl`, `index` -> `idx`, `dialog` -> `dlg`, etc. The argument that you don't have to type as much with shorter variable names is negated by autocompletion in an IDE, where all you have to do is type the first few letters and press tab (potentially an arrow key a couple times first). As for reading, humans read full words and sentences every day, so we're used to that. This makes it visually jarring for your brain to have to wrap around what `elmnt` or `chg` or `str` mean, forcing you to consciously think about (and consequently making it take longer for you to read) that name than if it were fully spelled out to `element` or `change` or `string`. Make names as easy to read and understand as possible.

That being said, there's a reasonable level of conciseness as well. It's sufficient to name something `clickedElement` instead of `htmlElementThatTheUserClickedOn`, or `applyFilter()` instead of `filterOutUnwantedItemsFromList()`. In between the two extremes is a balance with more subjective cases, like `shipmentPurchaseOrderApiService` being more verbose and specific, versus `shipmentService` being more general (but may conflict with a non-api shipment-related service). When in doubt, opt for being more verbose.

## Object-Oriented
### Access Modifiers
Access modifiers should always be specified. While class members default to different things in different languages, if the access modifier is unspecified, there are a few inherent advantages to specifying them: it makes it easier for other people to interact with your classes, and it makes it easier for you to interact with the code.

Traditional programming convention mandates information hiding, part of which includes making everything private except what absolutely needs to be public. This helps decouple objects from each other, trimming the dependency tree. When someone else is using your class, if a member doesn't pop up in the autocomplete suggestions, then they likely won't try to use it, resulting in fewer side effects. If someone else needs to use it, then they can change it to public when they need to. If you don't specifically intend for it to be public, make it private.

The second half of this advantage is for other people *reading* your code, rather than using it. If access members are specified, that clearly conveys your *intent*, rather than relying on the reader to assume your intent, when they're omitted.

Another readability aspect to specifiying access modifiers is the alignment visual consistency. When you're used to seeing them, it can be visually jarring seeing one missing, and you have to consciously think about what it defaults to in the language youre working in.

While writing code in an IDE, the "intellisense" (to use the Visual-Studio-specific name for general IDE behavior), will often change syntax-highlighting colors based on whether something is being used or is necessary. For example, unused or unreferenced imports, castings, variables, and parameters will be grayed out, as well as unnecessary uses of `this.` and namespace specifications (the `System.` in `System.Console`, for example, if `using System;` is specified). In Visual Studio, the same is true for unused *private* class members. Because a public class member could be used "indirectly" (in JS, this could be in an Angular template, indexed by name as a string, etc.), the editor doesn't always know whether it's actually referenced, so it assumes it is, and doesn't gray it out. Private class members, however, are assumed to not be referenced outside of the class, as per their intention, and are grayed out if not referenced, allowing you to easily visually identify (and remove/deprecate/otherwise handle) unused code that may be left over from previous refactoring.

The `readonly` keyword should also be used wherever possible and appropriate, to again convey intent to others reading your code. Note that this concept does not only apply to `private` and `public`, but also to `protected`, as well as `static`, and `abstract`.

### Strings
Most object-oriented languages have a string-interpolation equivalent, i.e. some way to inject expressions into string literals. These should be used wherever possible, since it's fewer characters and subjectively more readable than concatenating values. In some cases, it can also allow for automatic or condensed formatting syntax, particularly for date[time] values.

When returnings strings to the user, sometimes long strings can appear in the code. If there are several interpolations, one way to shorten it in the code is to create an array of the segments and join them together, rather than [arbitrarily] breaking and concatenating string segments. If it's just one big long string literal, it's easy enough to just scroll sideways to read it (see the editor section for easy ways to do that), since you rarely need to modify them, or read them in their entirety.

### Brackets/Braces
Whether a curly brace goes on the same line as the method/loop/if or on the next line depends on the language (see the section for the specific language), but across languages, they should always be specified. Most languages have a feature where in a control block like an `if` or a loop, if there's only one statement indented on the next line, then the braces are optional. This is not very mantainable for two reasons.

First, whitespace dependence is horrible for maintainability, as you have to spend time maintaining the whitespace instead of letting the editor or a linter do it for you, and it makes it easy for bugs to creep in, and potentially hard to track them down. Second, if you go to add another statement to the block, you then have to add in the braces. It's easier to maintain if you add the braces the first time than having to add them in later.

In addition, there's a readability argument to be made for always including them, due to consistency. If your eyes are used to seeing braces, then all of a sudden there's indentation without braces, it can be visually jarring to not see them, causing you to have to slow down and consciously think about what's happening, instead of it being visually obvious.

The one exception to this is inline callbacks. If a callback that solely returns the result of a single expression makes sense and is readable when it's inline, then the braces are optional. For example:
```ts
private findItemById(items: Item[], id: number): Item {
    return items.filter(item => item.id === id);
}
```

### Class Member/Variable Declaration/Initialization
Standard practice for object-oriented languages is to order classes in the following way: 1) fields/properties, 2) constructor(s), and 3) methods. The order within 1 and 3 is subjective, but typically public members are listed before private members. If subclasses are defined inside an outer class, it's often more readable to put those at the top of the outer class with the rest of the definitions, rather than at the bottom or arbitrarily in the middle.

Each type in a language gets a default value. While you can memorize which types get which defaults, you should always initialize variables and class members with a default value, even if it gets immediately reassigned the first time it's used. There are a couple advantages to doing that.

The first is conveying intent. While you may not necessarily care about the initial value of an `int` counter variable, for example, or while you may want it to start at zero, so you may let it initialize to the default, specifically initializing it to 0 conveys to the reader that you don't want it to start at 1, -1, or anything else. The same applies for strings, booleans, etc.

The second is error/edge-case handling. If you miss an edge case with some input, initializing to a specific default value can help you avoid crashes and runtime errors. You may still get unexpected behavior, but you'll be able to more easily see why.

In addition, in a dynamically-typed language like JavaScript, it's helpful for type enforcement to tell the editor what type a value is initially, for better autocompletion.

Whenever possible, initialize values when they're declared, rather than later. This makes the default value easy to find. 

## TypeScript/JavaScript
### General
If you're unfamiliar or not super confident with TypeScript, reading the [official documentation](https://www.typescriptlang.org/docs/) in its entirety is highly recommended. While lengthy, it will help tremendously with wrangling syntactic sugar that may otherwise look strange if you're coming from JS, as well as help understand the nuances of the terms used in this section.

### Semicolons
Semicolons, while technically optional, should always be used, for two reasons.

The first is consistency between languages. The traditional mainstream languages (C, C++, C#, Java, PHP, etc.) all require semicolons at the end of each statement, so JS should be no different. When using those other languages, your eyes are used to seeing semicolons, telling your brain that thats the end of a statement, so seeing one missing can be jarring and make you have to take a second look at a line to make sure it's not differently indented or something. Some people make the argument that it's easier to read code without semicolons; the counterargument is that it reinforces where statements start and end, improving readability.

The second reason is to avoid issues. Again, while technically optional, there are some cases where semicolons are necessary to avoid syntax errors or otherwise-unintended behavior.

### Quotes
This recommendation is one place where a language-specific standard overrides a guiding principle of this document, namely deviating from consistency between languages. Since JavaScript does not have a `char` type, it allows you to use either single or double quotes to define strings (along with backticks for interpolated strings). The precedent for JavaScript strings is to use single quotes, because historically, before template-driven frameworks became popular, it was common practice to have HTML snippets directly in the JS (most often in `<script>` tags), the standard practice for which is to use double quotes for attribute values. It was easier to just make the string in single quotes, rather than manually escaping every double quote in the enclosed HTML (or using single quotes for attribute values).

While in the context of JS frameworks this is almost always no longer applicable, many popular linters use single quotes by default, and it's still the more popular choice. In either case, consistency among all the JS/TS code is important; pick one and stick with it. While readability effects are minor in this case, the core priciple of consistency still applies.

Note that in accordance with the general-object-oriented language standards, interpolation should be used wherever applicable, in this case via backticks.

### Variable Declarations
When declaring variables, use `const` wherever you're not going to modify the value, `let` where you can't use `const`, and you shouldn't encounter any cases where you *need* to use `var` instead of `let`. You can Google block scope vs function scope, but the primary advantage to block scope is variable name reuse. In some cases it makes sense to have an inner variable that happens to have the same name as an outer-block variable:
```ts
private findItemById(items: Item[], id: number): Item {
    const item: Item = items.filter((item: Item): boolean => item.id === id);

    return item;
}
```

### Functions
In modern JS (>= ES6), there are two ways to define functions: regular functions and arrow functions. You can Google the differences, but in the context of class-based JS frameworks, the primary difference we encounter is the scope of `this`. Most functions in a framework like Angular are going to be either class members, in which case arrow functions don't apply, or callbacks. In a class-based context, it's favorable to use arrow functions in callbacks because it preserves the reference to `this`, as a reference to the surrounding class. That way, you can call methods and change fields inside a callback without having to worry about the scope of `this`.

### Type Specification
Return, variable, and parameter types should always be specified in TypeScript. Avoid `any` whenever possible, even if it means making a `type` for a single value. Writing in a dynamically-typed language, you want every advantage that the editor can give you while you're writing, to avoid runtime errors. Specifying types helps you catch errors when making changes, so you know what something else is expecting your function to return or your variable type to be.

    types vs interfaces
    import paths
    optional chaining
## C#
    null propagtion/optional chaining?
    conditional assignment
## HTML
    attributes [ordering]

## CSS/SCSS/SASS
    variables
    measurement units
    margin vs padding
    classes vs ids
    nested vs "flat" selector organization

## Angular
### CSS
    host/ng-deep

### TypeScript
    async vs observable
    constructor vs initialization
    component communication
    services
### HTML
    attributes
    interpolated values
    logic in template (getters, formatting, etc)




# Editors
## Visual Studio
### Useful Extensions
#### Side Scroll
Default Windows behavior allows for scrolling horizontally with the mouse wheel simply by holding shift, which is much faster than dragging a scollbar that may be hard to click. Shift-scroll isn't natively supported in Visual Studio for some reason, but there are extensions that enable it. There are different extensions for different versions of VS, but in those extensions, the other features (like any smooth scrolling) can be disruptive for coding, so they can be disabled, leaving only the shift-scroll.

### Shortcuts
A good programmer should always be looking for new keyboard shortcuts to avoid using the mouse. While going full Vim and keyboard-only isn't necessarily recommended, it's easy to make your workflow more optimal with a few handy keyboard shortcuts.

You can (and should) Google the full list for your editor of choice, but there are a few in Visual Studio that you should be using frequently:
- Auto-format: `ctrl+k+d` (this should become constant habit)
- Quick refactoring: `ctrl+.` or `alt+enter`
- Remove unused and sort imports/usings: `ctrl+r+g`
- Rename symbol: `ctrl+r+r`
- Go to definition: `F12` or `ctrl+click`
- Go to implementation : `ctrl+F12`
- Move line(s): `alt+down/up`
- Comment: `ctrl+k+c` (easier if rebound to `ctrl+/`)
- Uncomment: `ctrl+shift+k+c` (easier if rebound to `ctrl+shift+/`)

### Helpful Features
#### JavaScript Debugging

## VSCode
VSCode makes for a handy system-default text editor, being almost instant to start up, showing line numbers, easy settings, etc. It's often helpful to edit VS solution or project files, without confusing VS.
## SSMS
SSMS sort of has a dark theme, though it doesn't work in the sidebar. It's disabled by default, but a quick setting uncomment in a config file re-enables the option in the preferences: https://www.sqlshack.com/setting-up-the-dark-theme-in-sql-server-management-studio/

# Git
    commit messages
# Appendix
    JS syntax references
        - truth table
        - !!





| Rule | Reason |
| - | - |
| Header | Title |
| Paragraph | Text |



whitespace
    indentation
    new lines
    operator spacing
    braces/brackets
naming conventions
    files
    variables
    functions/methods
    classes
ordering
    imports
    class members
syntax
    access modifiers
    types
    quotes
    semicolons
semantics
    dry code
    single-responsibility
    class members
    coding choices
