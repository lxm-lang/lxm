# Index

- [Index](#Index)
- [Lexem modules](#Lexem-modules)
  - [Contexts](#Contexts)
  - [Imports](#Imports)
  - [Exports](#Exports)
    - [`pub` modifier](#pub-modifier)

# Lexem modules

Lexem modules work like Javascript's. Each Lexem file (`.lxm`) define a module which is isolated from the others and has their own dependencies.

Modules are imported and initialized only once so the rest of the imports just only get the module's public object.

## Contexts

A context is an object that contains all the variables of a piece of code (e.g. module, function, etc.). They have a parent context from which inherit all their variables creating an inheritance hierarchy.

Each module define its own context that directly inherits from the global context which is the only shared object across all imported modules.
The global context contains all the built-in components of the standard library.

Local contexts have a set of pre-defined **constants** that allow you to access to different elements of the hierarchy:

- `global`: the global context.
- `module`: the module context.
- `context`: the current context. In this case is the same as `module` but is shadowed when another element creates its own component, e.g. when inside function.
- `parent`: the parent of the current context. In this case is the same as `global` but is shadowed when another element creates its own component, e.g. when inside function.
- `exports`: the object that is returned when an import is made over this module.

> **Note**: `global` is equivalent to JavaScript's `window`, while `module` means the same and `exports` corresponds to `module.exports`.

## Imports

To import a module just call the `import` function, which is a built-in of the standard library, passing the name of the module context.

Imports can be local or remote, just specify the protocols `http` or `https` at the beginning.

```lexem
#- Relative
import("utils/extensions")

#- Absolute
import("/extensions")
import("c:/extensions")

#- Remote
import("http://github.com/lexemlang/")
import("https://github.com/lexemlang/")
```

## Exports

To import a module it is necessary that the module has previously exported its public components. The `exports` object must be filled with all elements that are public. For example:

```lexem
#- A variable
let x = 3

#- A function
fun funName() {}

#- The export
var exports = {
    x,
    funName,
}
```

But also it can be created at the beginning and progressively filled during the initialization of each element:

```lexem
#- The export (can be omitted)
var exports = {}

#- A variable
let x = 3
export.x = x

#- A function
fun funName() {}
export.funName = funName
```

The `export` variable initialization can be omitted due to Lexem creates it with a default plain object (`{}`).

### `pub` modifier

The latter is a common and useful pattern, so there is a modifier for statements that allows to simplify its writing:

```lexem
#- A variable
pub var x = 3

#- A constant
pub let y = "value"

#- A function
pub fun funName() {}

#- An expression
pub exp expName() {}
```

This code is interpreted like the previous section and can be used with a variable (`var`), a constant(`let`), a function (`fun`) or an expression (`exp`) statement declaration.