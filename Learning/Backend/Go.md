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
var a []string
fmt.Println(a, a == nil, len(a) == 0) // [] true true
```

To create an empty slice with non-zero length, builtin `make` is used. It creates a `slice` with provided length and zeroed values:
```go
s := make([]string, 3)
fmt.Println("s:", s, "len:", len(s), "cap:", cap(s)) // s: [  ] len: 3 cap: 3
```
By default, slice's capacity is equal to its length; it's possible to pass an additional capacity parameter to `make`, if different capacity is needed.

Get and set works just like with arrays:
```go
s := make([]string, 2)
s[0] = "a"
fmt.Println(s[0]) // a
```

In addition to these basic operations, `slices` support several more that make them richer than `arrays`. One is the builtin `append`, which returns a `slice` containing one or more new values. We need to accept a return value from `append` as we may get a new `slice` value:
```go
s := make([]string, 1)
s[0] = "a"
s = append(s, "b")
s = append(s, "c", "d")
fmt.Println(s) // [a b c d]
```

`Slices` can also be `copy`'d:
```go
var s []string
s = append(s, "a", "b")
l := make([]string, len(s))
copy(l, s)
fmt.Println(l) // [a b]
```

We can declare and initialise a variable for `slice` in a single line as well:
```go
s := []string{"a", "b", "c", "d", "e"}
```

`Slices` support a "slice" operator with the syntax `slice[low:high]`, `low` is included, and `high` is excluded:
```go
s := []string{"a", "b", "c", "d", "e"}
l := s[2:5]
fmt.Println(l) // [c d e]
```

`low` can be excluded to slice from 0 to the `high` (excluding it):
```go
s := []string{"a", "b", "c", "d", "e"}
l := s[:2]
fmt.Println(l) // [a b]
```

`high` can be excluded to slice from `low` (including it) to the end of the slice:
```go
s := []string{"a", "b", "c", "d", "e"}
l := s[3:]
fmt.Println(l) // [d e]
```

`slices` package contains useful utility functions for `slices`, e.g. `Equal`:
```go
s := []string{"a", "b", "c"}
l := []string{"a", "b", "c"}
if slices.Equal(s, l) {
	fmt.Println("s == l") // s == l
}
```

`Slices` can be composed into multi-dimensional data structures as well, but the length of inner `slices` can vary, unlike with multi-dimensional `arrays`:
```go
twoD := make([][]int, 3)
for i := 0; i < 3; i++ {
	innerLen := i + 1
	twoD[i] = make([]int, innerLen)
	for j := 0; j < innerLen; j++ {
		twoD[i][j] = i + j
	}
}
fmt.Println(twoD) // [[0] [1 2] [2 3 4]]
```

#### More on Arrays and Slices
Source: [Go Dev Blog](https://go.dev/blog/slices-intro)

Since `array` type definition specifies a length and element type, `array` types with the same element type but different lengths are distinct and incompatible (e.g. `[4]int` and `[5]int`).

In-memory representation of `[4]int` is just four integer values laid out sequentially:
![](Pasted%20image%2020241015200232.png)
Go's `arrays` are **values**. An `array` variable denotes the entire `array`; it is not a pointer to the first array element (as would be the case in C). This means that when you assign or pass around an array value you will make a copy of its contents. (To avoid the copy you could pass a *pointer* to the array, but then that's a pointer, not an array.) One way to think about arrays is as a sort of a struct but with indexed rather than named fields.

##### Slices
`Slices` are way more common in Go code. They build on arrays to provide great power and convenience.

Unlike an `array` type, a `slice` type has no specified length.

One of the ways to form a `slice` is by "slicing" an existing `slice` or `array`. 

##### Slice internals
A `slice` is a descriptor of an `array` segment. It consists of a pointer to the array, the length of the segment, and its capacity (the maximum length of the segment).
![[slice-struct.png]]

`s := make([]byte, 5)` is structured like this:
![[slice-1 (1).png]]

The length is the number of elements referred to by the `slice`. The capacity is the number of elements in the underlying `array` (beginning at the element referred to by the slice pointer).

If we were to slice `s` like so `s = s[2:4]`, the changes in the slice data structure would be the following:
![[slice-2.png]]

Slicing  does not copy the `slice`'s data. It creates a new slice value that points to the original `array`. This makes slice operations as efficient as manipulating array indices. Therefore, modifying the *elements* (not the `slice` itself) of a re-slice modifies the elements of the original slice:
```go
d := []byte{'r', 'o', 'a', 'd'}
e := d[2:] // e == []byte{'a', 'd'}
e[1] = 'm'
// e == []byte{'a', 'm'}
// d == []byte{'r', 'o', 'a', 'm'}
```

A `slice` cannot be grown beyond its capacity. Attempting to do so will cause a runtime panic. Similarly, slices cannot be re-sliced below zero to access earlier elements in the array.

##### Growing slices
To increase the capacity of a `slice` one must create a new, larger `slice` and copy the contents of the original `slice` into it. This is how dynamic array implementations from other languages work behind the scenes.

For example, to double (+2, in case it is zero-capacity) the capacity of a `slice` "s", we can do the following:
```go
t := make([]byte, len(s), (cap(s)+1)*2)
copy(t, s)
s = t
```

Another common operation is appending to a `slice`. This could be done by writing a custom utility function, which appends the needed values, and extends the `slice`'s capacity by using aforementioned approach if needed.

In most cases though, where complete control is not needed, Go provides a built-in `append` function.

To append one `slice` to another, `...` can be used to expand it to the list of arguments:
```go
a := []string{"John", "Paul"}
b := []string{"George", "Ringo", "Pete"}
a = append(a, b...) // equivalent to "append(a, b[0], b[1], b[2])"
// a == []string{"John", "Paul", "George", "Ringo", "Pete"}
```

##### A possible "gotcha"
Since re-slicing a `slice` doesn't make a copy of the underlying `array`, the full `array` will be kept in memory until it is no longer referenced. Occasionally this can cause the program to hold all the data in memory when only a small piece of it is needed.