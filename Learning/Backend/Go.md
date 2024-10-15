The following notes follow a condensed resource for people with prior programming knowledge: https://gobyexample.com/hello-world.

Running a Go program: `go run hello-world.go`.

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
Branching with `if` and `else` in Go is straightforward:

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
`switch` statements express conditionals across many branches.

A basic switch statement looks like this:
```go
i := 2
switch i {
case 1:
	fmt.Println("one")
case 2:
	fmt.Println("two")
case 3:
	fmt.Println("three")
}
```

You can use commas to separate multiple expressions in the same `case` statement. You can also provide a `default` case:
```go
switch time.Now().Weekday() {
case time.Saturday, time.Sunday:
	fmt.Println("It's the weekend")
default:
	fmt.Println("It's a weekday")
}
```

`switch` without an expression is an alternate way to express if/else logic:
```go
t := time.Now()
switch {
	case t.Hour() < 12:
		fmt.Println("It's before noon")
	default:
		fmt.Println("It's after noon")
}
```

A type `switch` compares types instead of values. It can be used to discover type of an interface value:
```go
whatAmI := func(i interface{}) {
	switch t := i.(type) {
		case bool:
			fmt.Println("I'm a bool")
		case int:
			fmt.Println("I'm an int")
		default:
			fmt.Println("Don't know type %T\n", t)
	}
}
whatAmI(true) // I'm a bool
whatAmI(1) // I'm an int
whatAmI("hey") // Don't know type string
```

#### Arrays
In Go, an `array` is an indexed sequence of elements of a specific length. In typical Go code, `slices` are much more common; arrays are useful in some special scenarios, e.g:
- **Fixed-size data structures**: e.g. a matrix with fixed dimensions.
- **Performance-critical code**: `arrays` can be slightly faster, as they don't require an overhead associated with `slices` (such as maintaining a separate header with the length and capacity).
- **Memory constraints**: `arrays` are typically allocated on the stack, whereas `slices` typically involve heap allocation, which means `arrays` can be more efficient in terms of memory access times and garbage collection. This can be crucial in low-memory environments, such as embedded systems.
- **Fixed-length hashing or encoding algorithms**: Many cryptographic algorithms (such as SHA-256) operate on fixed block sizes.
- **Interop with C libraries**: If you are using Go's `cgo` to interact with C libraries, certain functions might require fixed-size `arrays`.

Here's an example of an array that will hold exactly 5 `ints`. The type of elements and the length are both part of the `array`'s type. By default, the array is zero-valued:
```go
var a [5]int
fmt.Println(a) // [0 0 0 0 0]
```

We can set a value using `array[index] = value` syntax, and get it with `array[index]`:
```go
a[4] = 100
fmt.Println(a[4]) // 100
```

The built-in `len` returns the length of an `array`:
```go
var a [5]int
fmt.Println(len(a)) // 5
```

You can declare and initialise an array in one line with this syntax:
```go
b := [5]int{1, 2, 3, 4, 5}
```

You can have the complier count the elements with `...`:
```go
b := [...]int{1, 2, 3, 4, 5}
```

If you specify the index with `:`, the elements in-between will be zeroed:
```go
b := [...]int{100, 3: 400, 500}
fmt.Println(b) // [100 0 0 400 500]
```

Array types are one-dimensional, but you can compose types to build multi-dimensional data structures:
```go
var twoD [2][3]int
for i := 0; i < 2; i++ {
	for j := 0; j < 3; j++ {
		twoD[i][j] = i + j
	}
}
fmt.Println(twoD) // [[0 1 2] [1 2 3]]
```

You can create and initialise multi-dimensional arrays at once too:
```go
twoD := [2][3]int{
	{0, 1, 2},
	{1, 2, 3},
}
fmt.Println(twoD) // [[0 1 2] [1 2 3]]
```

#### Slices
`Slices` are an important data type in Go, giving more powerful interface to sequences than `arrays`.

Unlike `arrays`, `slices` are only typed by the elements they contain, not the number of them. An uninitialised `slice` equals to `nil` and has the length 0:
```go
var a = []string
fmt.Println(a, a == nil, len(a) == 0) // [] true true
```

