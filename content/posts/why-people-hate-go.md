+++ 
draft = false
date = 2025-07-23T14:16:55-03:00
title = "Why People Love to Hate Go"
description = "Go has good ideas but never gives them to you completely."
slug = ""
authors = ["Jack Mordaunt"]
tags = ["Go", "Odin"]
categories = ["development", "opinion"]
externalLink = ""
series = []
+++

> Go is very idiosyncratic, approximating good ideas and relying heavily on data transposition.

In this opinion article I will higlight why I think people love to hate Go.

I'll do so by contrasting Go to Odin. Odin serves as an excellent comparison because the
syntax is similar in essence (both heavily inspired by Pascal and C).

Unlike Go, Odin actually gives you the good ideas in their completeness -
without a complex type system or giving up any control, and without needing to
fight the ideas for a decade only to finally concede.

This opinion is not a knock against Go authors, or community of which I respect and have
been a part for a long time.

Modern versions of Go have addressed most of the examples highlighted here.
This serves to reinforce the underlying of the article: that these things
are valuable, proven by the fact that Go eventually conceded on them.

- Go now has generics
- Go now has iterators
- Go now has an `any` alias that is more clear naming than `interface{}`

What follows is not an exhaustive list, but rather just highlights to frame the point being made.

## Generics for Me but Not for Thee

> Now, time to beat the horse some more.

Generics: when the relationship _between_ data is the thing you are trying to
encode, not the shape of the data itself.

### Go

For the longest time Go had no user-level generics.

It did have generics, but only for the runtime!

This is ironic, because it proves the utility of generics while at the very same
time denying it to the user.

Go has relied heavily on the transpositional nature of data, using one form to represent another.
It graces you with the most important structures, which alleviates the problem:

- array
- pointer
- map
- slice            (dynamic array)
- channel          (thread-safe queue)
- closure          (anonymous function)
- multiple-returns (pseudo tuple)

In many cases you can rely on what I dub "lexical generics", function closures that capture
lexical values implicitly capture their types as well - making closures highly type-generic
in a fascinating way. You will see this be {ab,}used in many Go codebases.

Often the friction of custom data structures is high enough that the desired data structure
will be transposed onto the runtime data structures.

Do you want a queue or stack? Use a slice.
Do you want an iterator? Use a channel.
Do you want a set or graph? Use a map.
Do you want an enum? Use an integer.
Do you want a tagged union? Use an integer kind and a fat struct.

As you can imagine these choices can come with significant penalties. Using a channel as
an iterator, simply because it is both type-generic and integrates with the looping syntax, adds
performance overhead due to its thread safety properties. If you don't need threadsafety, you're
needlessly paying for it anyway, just to coerce the language into being ergonomic to you.

It's not a virtue to rely on the transpositional nature of data structures to avoid generics,
but this was the underlying argument for "why do you even need generics?".
The true formulation is more like "we have given you enough generic structures,
just transpose onto them".

Go never really told you that generics was not needed, only that it had provided enough already.

### Odin

Odin gives you parametric polymorphism: a simple form of generics that allows for the building of
type-safe, generic data structures. It is "simple" because it doesn't come with a lot of features
or baggage. There are no traits, no method sets, and no "complex type system".

Parametric: of parameters.
Polymorphism: of many shapes.

Put together it means "parameter that can take on many shapes". The Odin
community shortens this to "parapoly" for brevity.

When people say they want generics, parapoly is usually what they want.

The reality: types are a compile-time-known datum. The idea that you cannot
parameterise structures and functions by a simple shape known at compile time
is silly. The problem domain: not very complex. This will rhyme when we talk about
dynamic types.

The punchline: Odin gives you what you actually wanted, parametric polymorphism.
It doesn't expect you to transpose yourself into oblivion.

```odin
// A simple structure that cares about the relationship between T's,
// not the shape of T itself. Tell me, where is the complex type-system?
Node :: struct($T: typeid) {
  parent: ^T,
  child:  ^T,
}
```

## No True Dynamic Type

> Noob: How do I do dynamic typing in Go?
>
> Go: Here, use this: `interface{}` - it's called the "empty interface".
>
> Noob: ...
>
> Noob: Uh, what? 
>
> Go: I wasn't really meant for dynamic typing, use that or structure your program differently.
>
> Noob: Ok fine, I'll use the "empty interface" then...
>

Dynamic programming is important and useful, it's also not difficult to support first-class.

Dynamic types reduce to a fat pointer: a structure that contains two pointers,
one pointing to the data and one pointing to the type information. This allows
the program to handle arbitrary values at runtime by writing logic against
dynamic types.

However Go is an OOP language. It encodes the very essence of OOP: behavioural polymorpshim
based on v-tables. The term used in Go for this concept is the `interface`.

Like a true dynamic type, the interface is indeed a fat pointer:

```go
type ITab struct {
	Inter *InterfaceType
	Type  *Type
  // --snip--
}
```
The revealing part is the separate definition for the empty interface:

```go
type EmptyInterface struct {
	Type *Type
	Data unsafe.Pointer
}
```

Why is this bothersome? Instead of just giving you an "any" type,
Go forces you to talk in terms of "interfaces that have no methods".

Linguistically and conceptually this makes no sense. 

This is philosphically impure.

Look at these beauties:
- `[]interface{}`
- `map[interface{}]interface{}`
- `func(string, ...interface{})`
- `struct { value interface{} }`

A better approach is to do what Odin does: just give you the `any` type.
Defined as simple as the concept is, a fat pointer.

```odin
// Raw_Any points at some data, and associates type data with it.
// This type is named `any` at the user-level.
Raw_Any :: struct {
	data: rawptr,
	id:   typeid,
}
```

Once you understand fat pointers, dynamic programming becomes easy.

Now, if you want an interface in Odin, you simply define a struct that contains
a pointer to the data and a pointer to the implementation procedure (rather than
a typeid).

Consider the common allocator interface:

```odin
Allocator :: struct {
	procedure: Allocator_Proc,
	data:      rawptr,
}
```

It's the same underlying idea (with a v-table of 1 to keep it ergonomic).

Any specialised allocator just needs to map itself onto this generic Allocator
structure and voilà, you have achieved runtime behavioural polymorphism!

Go has rectified the language-side of this in later releases, including the
`any` alias (equivalent to `interface{}` in all ways), but you can see how
Go's design doesn't reason up from first principles, but rather down from empiricism.

> We think objects are good idea.
>
> We want to do them at runtime.
>
> Aha, the interface type. It's a fat pointer to a vtable and some data.
>
> Look, if you want to do dynamic programming you can just use an empty interface!
>

## Declaration and Assignment Syntax

Go and Odin both use this Pascal inspired declaration-assignment syntax `:=`.

```go
// Go

var variable string = "variable"
var variable = "variable"

const constant string = "constant"
const constant  = "constant"

x := "foobar"
```

```odin
// Odin

variable := "variable"
variable: string = "variable"

constant :: "constant"
constant: string : "constant"

x := "foobar"
```

What's the problem? Well, the thing is that Go defines `:=` as a keyword.
You cannot actually place a type between `:` and `=`.

Once again Go fails to give you the complete idea, the grammar: `<symbol> : <type> = <expression>`.
Where the type can be elided to result in `<symbol> := <expression>`.
Where constants are the same, but with a second `:` instead of `=`.

Go has chosen an inconsistent approach. When the underlying grammar is elegant,
Go decides to take the shortcut and ignore the point. Odin just gives you the
elegant, consistent grammar.

Consider type and proc definitions:

```go
// Go

type Node struct {
	// --snip--
}

func Visit(n Node) {
	// --snip
}
```

In the Go syntax we have introduced more keywords to declare types and
functions. Why do we need extra syntax here? After all, struct and function
definitions are just regular constants!

In Odin, this fact is evident.

```odin
// Odin

node :: struct {
	// --snip--
}

visit :: proc(n: node) {
	// --snip--
}
```

The syntax is consistent:
- consistent with other constants
- consistently read left-to-right, with symbol first, type second, and finally the binding third

After all, a struct defintion is a compile-time known value.
Thus, the `node` type is just regular constant set to a typeid.

Don't believe me? Check this out:

```odin
// Odin

node: struct {} : struct {
	// --snip--
}

visit: proc(n: node) : proc(n: node) {
	// --snip--
}
```

You _can_ put the type in the normal type position, it's just elided by convention!

And if you want them to be variables instead:

```odin
// Odin

node: struct {} = struct {
	// --snip--
}

visit: proc(n: node) = proc(n: node) {
	// --snip--
}
```

Odin takes the underlying idea of elegant, minimal and consistent grammar and
actually just hands it to you. Type definitions are _just_ constants!

Go decides to keep the shorthand `:=` but never actually fully embraces the
underlying idea. You can't put a type between `:` and `=`, and you are expected
to use bespoke keywords for type and function declarations.

Feel the inconsistency.

```go
// Go
const c int = 42
const c = 42
var v int = 42
var v = 42
v := 42
```

```odin
// Odin
c: int : 42
c :: 42
v: int
v: int = 42
v := 42
```

## Conclusion
Go develops features empirically, adopting what works practically. Odin constructs from first principles, focusing on conceptual elegance.

Go’s incremental evolution led to ad-hoc syntax and features, making it feel less coherent to users accustomed to functional or fully featured OOP languages.

Conversely, Odin feels purposeful and coherent, designed from foundational ideas rather than incremental adaptations.

Despite these criticisms, Go remains practical and solid, which explains its continued popularity—and why many developers both love and hate it.
