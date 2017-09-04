## Packages
Every Go program is made up of packages.

Programs start running in package main.
```go
package main
```

This program is using the packages with import paths "`fmt`" and "`math/rand`".

## Imports
This code groups the imports into a parenthesized, "factored" import statement.
```go
import (
	"fmt"
	"math"
)
```

## Exported Names
In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, 
which is exported from the math package.

`pizza` and `pi` do not start with a capital letter, so they are not exported.

## Functions
A function can take zero or more arguments.
```go
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

In this example, add takes two parameters of type `int`.

Notice that the type comes **after** the variable name.

When two or more consecutive named function parameters share a type, you can omit the type from all but the last.

```go
func add(x, y int) int {
	return x + y
}
```

## Multiple results
A function can return any number of results.

The swap function returns two strings.

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

### Named return values
Go's return values may be named. If so, they are treated as variables defined at the top of the function.
```go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
```

These names should be used to document the meaning of the return values.

A return statement without arguments returns the named return values. This is known as a "naked" return.

## Variables
The var statement declares a list of variables; as in function argument lists, the type is last.
```go
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```
### Variable Initializers
```go
var i, j int = 1, 2
var c, python, java = true, false, "no!"
```
### Short variable declarations in functions
Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.
```go
func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

### Zero values
The zero value is:
- `0` for numeric types,
- `false` for the boolean type, and
- `""` (the empty string) for strings.

## Type conversions
The expression `T(v)` converts the value `v` to the type `T`.
```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

i := 42
f := float64(i)
u := uint(f)
```

## Type inference
When declaring a variable without specifying an explicit type (either by using the := syntax or var = expression syntax), the variable's type is inferred from the value on the right hand side.

When the right hand side of the declaration is typed, the new variable is of that same type:
```go
var i int
j := i // j is an int
```
But when the right hand side contains an untyped numeric constant, the new variable may be an `int`, `float64`, or `complex128` depending on the precision of the constant:
```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

## Constants
Constants are declared like variables, but with the const keyword.

Constants can be character, string, boolean, or numeric values.
```go
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

## Loops
Go has only one looping construct, the `for` loop.

The basic for loop has three components separated by semicolons:
- the init statement: executed before the first iteration
- the condition expression: evaluated before every iteration
- the post statement: executed at the end of every iteration

The loop will stop iterating once the boolean condition evaluates to `false`

Note: Unlike other languages like C, Java, or Javascript there are no parentheses surrounding the three components of the `for` statement:
```go
	sum := 0
	for i := 0; i < 20; i++ {
		sum += i
	}
```

The init and post statement are optional.
```go
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
```
### Go equivalent of `while` loop
```go
	sum := 1
	for sum < 1000; {
		sum += sum
	}
```
### Infinity loop in Go
```go
    for {
    }
```
## Conditions
Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses `( )` but the braces `{ }` are required.
```go
	if x < 0 {
		return sqrt(-x) + "i"
	}
```
Like `for`, the `if` statement can start with a short statement to execute before the condition.
Variables declared by the statement are only in scope until the end of the `if`.
```go
	if v := math.Pow(x, n); v < lim {
		return v
	}
```
Variables declared inside an `if` short statement are also available inside any of the `else` blocks.
```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}
```
### switch
A case body breaks automatically, unless it ends with a `fallthrough` statement.
```go
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}
```
Switch without a condition is the same as `switch true`.

This construct can be a clean way to write long if-then-else chains.
```go
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
```

## Defer
A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.
```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
// > hello
// > world
```
Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.
```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
// > counting
// > done
// > 9
// ...
// > 0
```

-------------
## References
[A Tour of Go](https://tour.golang.org/)

```go

```