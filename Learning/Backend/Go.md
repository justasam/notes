The following notes follow a condensed resource for people with prior programming knowledge: https://gobyexample.com/hello-world.

Running a go program: `go run hello-world.go`.

Building a binary: `go build hello-world.go`.

#### Values
Go has various value types including strings, integers, floats, complex numbers, booleans, etc.

Strings can be added together with `+`.
```go
fmt.Println("go" + "lang") // "golang"
```

#### Variables
In Go, variables are explicitly declared and used by compiler to e.g. check type-correctness of function calls.

`var` declares 1 or more variables.
```go
var a = "initial"
```

You can declare multiple variables at once.
```go
var b, c int = 1, 2 
```

Go will infer the type of initialised variables.
```go
var d = true
```

Variables declared without a corresponding initialisation are _zero-valued_.
```go
var e int // e == 0
```

The `:=` syntax is shorthand for declaring and initialising a variable. This syntax is only available within functions
```go
func main() {
	f := "Fig" // var f string = "Fig"
}
```

#### Constants
Go supports _constants_ of character, string, boolean, and numeric values.

`const` declares a constant value.
```go
const s string = "constant"
```
A `const` statement can appear anywhere a `var` can.

Constant expressions perform arithmetic with arbitrary (infinite) precision.
```go
const d = 3e20 / 500000000 // 6e+11
```

A numeric constant has no type until it's given one.
```go
func main() {
	const d = 6e+11
	fmt.Println(d) // 6e+11
	fmt.Println(int64(d)) // 600000000000
}
```

A numeric constant can be given type by using it in context that requires one, such as variable assignment or function call.
```go
func main() {
	const n = 500000000

	fmt.Println(math.Sin(n)) // -0.28470407323754404 -- a float64
}
```

#### For
`for` is Go's only looping construct.
There are different types of `for` loops:

- Single condition `for` loop
```go
var i int = 0

for i <= 3 {
	fmt.Println(i)
	i = i + 1	
}
```
- Initial/condition/after `for` loop
```go
for j := 0; j < 3; j++ {
	fmt.Println(j)
}
```
- `range` `for` loop
```go
for i := range 3 {
	fmt.Println(i)
}
```
- infinite `for` loop
```go
for {
	fmt.Println("I am infinite")
	break // unless this happens.
}
```

`break` and `continue` are available in `for`  loops, allowing to break out of the loop or continue to the next iteration, respectively.

#### If/Else
Branching with `if` and `else` in go is straightforward:

```go
if 7%2 == 0 {
	fmt.Println("7 is even")
} else {
	fmt.Println("7 is odd")
}
```

A statement can precede conditionals; any variables declared in the statement are available in the current and subsequent branches:
```go
if num := 9; num < 0 {
	fmt.Println(num, "is negative")
} else if num < 10 {
	fmt.Println(num, "has 1 digit")
} else {
	fmt.Println(num, "has multiple digits")
}
```

You can have an `if` statement without an `else`.

You don't need parentheses around conditions, but the braces are required.

There is no `ternary if` in go, so you'll need to use full `if` statement even for basic conditions.

#### Switch
