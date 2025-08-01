+++ 
draft = false
date = 2025-08-01T06:26:18-03:00
title = "What is an Interface"
description = "Behavioral Polymorphism: The foundation of OOP."
slug = ""
authors = []
tags = ["OOP", "Go", "Odin", "Interface"]
categories = []
externalLink = ""
series = []
+++

**Behavioral Polymorphism: The foundation of OOP.**

The **interface** is the foundation of OOP (Object Oriented Programming).

It provides _behavioral polymorphism_: constraining a type by its behaviours -
or more secifically, its method-set.

> A "method" (as termed in OOP langauges) is nothing more than a procedure who's
> first argument is a pointer to the object it is associated with. There is
> nothing special about a method.

**Definitions**

- **Poly**: many
- **Mono**: singular
- **Morphism**: shape
- **Polymorphism**: having many shapes
- **Monomorphism**: having one shape

Polymorphic refers to the ability to represent many shapes.

**Anology: Power Sockets**

You might have ten electrical appliances all completely different.
Toaster, microwave, blender.
The way they use the electricity are all different, but the power plug is the same.
The power socket provides a common interface that allows polymorphism with respect to the appliances.
As long as an appliance has a the correct plug, the power socket will accept it.
Thus the power socket is an interface that enables behavioral polymorphism.

Polymorphism in programing works the same way.

> In dynamically-typed programming every value is polymorphic, and only the _use_ of the value will reveal whether the particular morphism is valid.
> In statically-typed programming every value is monomorphic, and special types must be used to allow for polymorphism.

**What is a shape anyway?**

In programming a shape is some definable characteristic about a value.
- At a low level: how are the fields represented in memory
- At a high level: is it iterable, is it indexable, is it a reference, etc

We abstract the low level up to the high level.

OOP languages often build in language-level native support for interfaces since
this kind of polymorphism is foundational to the OOP paradigm. In low level
languages like C you will typically achieve polymorphism through data layout:
using flexible structures and memory indirection.

Interfaces allow for polymorphism in statically-typed languages.
That is, they don't require generics in the type-system to implement.

It is no coincidence that Java and Go started out statically-typed using interfaces.
Lacking parametric polymorphism, those languages often awkwardly relied on behavioral polymorphism.

## The Fat Pointer

This section applies to _runtime_ interfaces, otherwise known as _dynamic dispatch_.

At the low level, an interface is nothing more than a **fat pointer**: a structure
containing a pair of pointers - one that points to the data, and the other that
points to a function that operates on the data.

This is all you need.

**Odin Example: Stream**

```odin
Stream_Proc :: #type proc(stream_data: rawptr, mode: Stream_Mode, p: []byte, offset: i64, whence: Seek_From) -> (n: i64, err: Error)

Stream :: struct {
	procedure: Stream_Proc,
	data:      rawptr,
}
```

This struct contains a pointer to a procedure and a pointer to the data.
It can be used to represent any kind of streaming operation: reading, writing, flushing, closing, etc, on any type.

Each specialized type (file, buffer, http body) simply needs to map itself to this shape and it can integrate with streaming logic.

**Converting a Buffer to a Stream**

```odin
buffer_to_stream :: proc(b: ^Buffer) -> (s: io.Stream) {
	s.data = b
	s.procedure = _buffer_proc
	return
}
```

It simply builds the stream struct using a pointer to the buffer sets the correct procedure.

To achieve behavioral polymorphism all you need is a common
representation (the fat pointer) that all specialized types can conform to.

**Usage Example: io.Reader**

```odin
package main

import "core:bytes"
import "core:io"
import os "core:os/os2"

main :: proc() {
	// Assign both the file and buffer reader to this to show they are the same shape.
	reader: io.Reader

	// Scratch is just a buffer to read into.
	scratch := make([dynamic]byte, 1024 * 1024)

	// Open a file and make a reader for it.
	file, _ := os.open("file")
	reader = os.to_reader(file)

	// This call is operating on the file.
	io.read_full(reader, scratch[:])

	// Allocate a byte buffer and make a reader for it.
	buf: bytes.Buffer
	bytes.buffer_init(&buf, make([dynamic]byte, 1024 * 1024)[:])
	reader = bytes.buffer_to_stream(&buf)

	// This call is operating on the buffer.
	io.read_full(reader, scratch[:])
}
```

In this example, the `io.Reader` is the interface.
The `bytes.Buffer` and the `os.File` are the specialized types can map to it.
`io.read_full` can therefore operate on any type that can map to an `io.Reader`.

Behavioral polymorphism is achieved by matching shapes. No runtime or type system
support needed.


## The VTable

**Virtual Table**

The Odin core prefers to (but doesn't require) to use a VTable-of-one. A single
procedure that can handle any possible operation of the interface.

This keeps the shape a simple fat-pointer, making it easier to construct and
more compact in memory.

The traditional way (e.g., C++) assigns each method its own entry in the VTable.
This means the pointer to the procedure becomes a pointer to a table of procedures.

Each entry into the virtual table represents a single method.

**Example: Multi-Method VTable**

```odin
FileInterface :: struct {
  vtable: FileVTable,
  data: rawptr,
}

FileVTable :: struct {
  read: proc(data: rawptr, p: byte[]) -> (n: int, err: Error),
  write: proc(data: rawptr, p: byte[]) -> (n: int, err: Error),
  close: proc(data: rawptr) -> Error,
}
```

As you can see, each additional method increases the memory required.

There is no **logical** difference between the two representations, only structural.

## Windows COM

To see how far you can take behavioral polymorphism, look no further than Windows COM.

COM is an API and ABI implemented entirely around VTables — to the exclusion of all else.

You can call and implement a COM object in _any_ langauge, as long as the VTable
representation and behaviour of the methods are correct.

The point is not to teach COM, but to illustrate that interfaces are just VTables.
Behavioral polymorphism is a simple low-level idea that is abstracted into OOP
languages as a first class language feature.

Windows COM also shows how flexible the humble VTable can be — it allows a
polyglot object system. Fascinating, but probably not something to aspire to.

## The Cost

Interfaces are simple structures, but they are not free.

- **Size**: at a minimum, two pointers.
- **Growth**: VTable size grows with number of methods.
- **Indirection**: Runtime method calls are indirect.

In large dynamic arrays, interface overhead can add up:
- **Memory cost**
- **Cache pressure** due to pointer indirection
- **Fragmented memory** for the actual data

With a more sophisticated type system, interfaces can be monomorphised at
compile-time (i.e., static dispatch).

This eliminates runtime cost but increases compile-time complexity. 

## Conclusion

Interfaces allow behavioral polymorpshim.

The classic runtime interface is implemented as a simple fat-pointer shape.

Concrete types only need to satisfy the shape of the interface and they can be used anywhere
that interface is used.

Interfaces are not scary, and not hard to implement at the low level. They are also not a
substitute for parametric polymorphism as I'm sure many Go programmers are well aware.
