# add

Add two numbers with `avo`.

The [code generator](asm.go) is as follows:

[embedmd]:# (asm.go)
```go
// +build ignore

package main

import . "github.com/mmcloughlin/avo/build"

func main() {
	TEXT("Add", NOSPLIT, "func(x, y uint64) uint64")
	Doc("Add adds x and y.")
	x := Load(Param("x"), GP64())
	y := Load(Param("y"), GP64())
	ADDQ(x, y)
	Store(y, ReturnIndex(0))
	RET()
	Generate()
}
```

We use a [`go:generate`](https://blog.golang.org/generate) line to produce the assembly and Go stub files together.

[embedmd]:# (add_test.go go /.*go:generate.*/)
```go
//go:generate go run asm.go -out add.s -stubs stub.go
```

This produces [`add.s`](add.s) as follows:

[embedmd]:# (add.s)
```s
// Code generated by command: go run asm.go -out add.s -stubs stub.go. DO NOT EDIT.

#include "textflag.h"

// func Add(x uint64, y uint64) uint64
TEXT ·Add(SB), NOSPLIT, $0-24
	MOVQ x+0(FP), AX
	MOVQ y+8(FP), CX
	ADDQ AX, CX
	MOVQ CX, ret+16(FP)
	RET
```