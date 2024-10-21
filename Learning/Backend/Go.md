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

![In-memory representation](../../Note%20Pictures/Pasted%20image%2020241015200232.png)

Go's `arrays` are **values**. An `array` variable denotes the entire `array`; it is not a pointer to the first array element (as would be the case in C). This means that when you assign or pass around an array value you will make a copy of its contents. (To avoid the copy you could pass a *pointer* to the array, but then that's a pointer, not an array.) One way to think about arrays is as a sort of a struct but with indexed rather than named fields.

##### Slices
`Slices` are way more common in Go code. They build on arrays to provide great power and convenience.

Unlike an `array` type, a `slice` type has no specified length.

One of the ways to form a `slice` is by "slicing" an existing `slice` or `array`. 

##### Slice internals
A `slice` is a descriptor of an `array` segment. It consists of a pointer to the array, the length of the segment, and its capacity (the maximum length of the segment).

![Slice struct](../../Note%20Pictures/slice-struct.png)

`s := make([]byte, 5)` is structured like this:

![Slice of 5 bytes struct](../../Note%20Pictures/slice-1%20(1).png)

The length is the number of elements referred to by the `slice`. The capacity is the number of elements in the underlying `array` (beginning at the element referred to by the slice pointer).

If we were to slice `s` like so `s = s[2:4]`, the changes in the slice data structure would be the following:

![Sliced slice structure](../../Note%20Pictures/slice-2.png)

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

#### Maps
`Maps` are Go's built-in *associative data type* (sometimes called *hashes* or *dicts* in other languages).

To create an empty map, builtin `make` should be used (`make(map[key-type]val-type)`):
```go
m := make(map[string]int)
```

`name[key] = val` syntax is used to set key/val pairs:
```go
m["k1"] = 7
```

Printing a map will show all of its key/value pairs:
```go
m := make(map[string]int)
m["k1"] = 7
m["k2"] = 14
fmt.Println("map:", m) // map: map[k1:7 k2:14]
```

If the key doesn't exist, the *zero value* of the value type is returned.

The builtin `len` returns the number of key/value pairs when called on a `map`.

The builtin `delete` removes key/value pairs from a `map`.

The builtin `clear` removes *all* key/value pairs from a `map`.

The optional second return value when getting a value from a `map` indicates if it has the key. Unneeded value can be ignored with the *blank identifier* `_` (in this case, the value itself):
```go
m := make(map[string]int)
_, hasKey := m["k5"]
fmt.Println(hasKey) // false
```

A `map` can be declared and initialised in the same line:
```go
n := map[string]int{"foo": 1, "bar": 2}
```

The `maps` package contains a number of useful utility functions for `maps`:
```go
import (
	"fmt"
	"maps"
)

n := map[string]int{"a": 1, "b": 2}
n2 := map[string]int{"a": 1, "b": 2}
if maps.Equal(n, n2) {
	fmt.Println("n == n2") // n == n2
}
```

#### Functions
`Functions` are central in Go. 

Here's a `function` which takes two `ints` and returns their sum as an `int`:
```go
func plus(a int, b int) int {
	return a + b
}
```
Go requires explicit returns -- it won't automatically return the value of the last expression.

When multiple consecutive parameters have the same type, it can be omitted up to the final parameter, which declares the type:
```go
func plusPlus(a, b, c int) int {
	return a + b + c
}
```

A function can be called with `name(args)` syntax:
```go
sum := plusPlus(1, 4, 5) // sum == 10
```

##### Multiple Return Values
Go has builtin support for *multiple* return values. This feature is often used in idiomatic Go (writing Go code which follows the conventions, patterns and best practices), for example to return both result and error values from a function.

The `(int int)` in this function signature shows that the function returns two `ints`:
```go
func vals() (int, int) {
	return 3, 7
}
```

We can use the different return values with *multiple assignment*:
```go
a, b := vals()
```

If only a subset of return values is needed, *blank identifier* (`_`) can be used:
```go
_, c:= vals()
```

##### Variadic Functions
*Variadic functions* can be called with any number of trailing arguments. For example, `fmt.Println` is a common variadic function.

To define a variadic function, you should use `...` before the type of the last argument. Inside the function, the arguments are treated as a `slice` of that type:
```go
func sum(nums ...int) {
	total := 0
	for _, num := range nums {
		total += num
	}
	fmt.Println("sum:", total)
}
```

If you already have multiple args in a `slice`, you can apply them to a variadic function using `func(slice...)` like this:
```go
nums := []int{1, 2, 3, 4}
sum(nums...) // sum: 10
```

##### Closures
Go supports *anonymous functions*, which can form *closures*. Anonymous functions are useful when you want to define a function inline, without having to name it.

This function `intSeq` returns another function, which is defined anonymously in the body of `intSeq`. The returned function *closes over* the variable `i` to form a *closure*:
```go
func intSeq() func() int {
	i := 0
	return func() int {
		i++
		return i
	}
}

func main() {
	nextInt := intSeq()
	fmt.Println(nextInt()) // 1
	fmt.Println(nextInt()) // 2

	newInts := intSeq()
	fmt.Println(newInts()) // 1
}
```

##### Recursion
Go supports *recursive functions*, i.e. functions which call themselves.

Here's an example of a recursive function, which calls itself until it reaches the base case of `fact(0)`:
```go
func fact(n int) int {
	if n == 0 {
		return 1
	}
	return n * fact(n-1)
}
```

*Anonymous functions* can also be recursive, but this requires explicitly declaring a variable with `var` to store the function before it's defined:
```go
var fib func(n int) int

fib = func (n int) int {
	if n < 2 {
		return n
	}

	return fib(n-1) + fib(n-2) // since fib was previously declared, Go knows which function to call with fib here.
}
```

#### Range over Builtin Types
`range` iterates over elements in a variety of builtin data structures.

Here's how `range` is used to iterate numbers in `arrays` or `slices`:
```go
nums := []int{2, 3, 4}
sum := 0
for index, num := range nums {
	sum += num
}
// sum == 9
```
`range` on `arrays` and `slices` provides both the index and value for each entry. If any of these is not needed, it can be ignored with the blank identifier `_`.

`range` on `map` iterates over key/value pairs:
```go
kvs := map[string]string{"a": "apple", "b": "bannana"}
for k, v := range kvs {
	fmt.PrintF("%s -> %s\n", k, v) // a -> apple
}
```

`range` can also iterate over just the keys of a `map`:
```go
for k := range kvs {
	fmt.Println("key:", k) // key: a
}
```

`range` on `strings` iterates over Unicode code points. The first value is the starting byte index of the `rune` and the second is the `rune` itself:
```go
for i, c := range "go" {
	fmt.Println(i, c)
}
// 0 103
// 1 111
```

#### Pointers
Go supports `pointers`, allowing to pass references to values and records.

Non-pointer function parameters have arguments passed to them by value:
```go
func zeroval(ival int) { // passed by value
	ival = 0
}
i := 1
zeroval(i)
fmt.Println("zeroval:", i) // zervoal: 1
```

In contrast, `pointer` function parameters receive a memory address of the value. The pointer can then be _dereferenced_, and a new value can be assigned to that address.
```go
func zeroptr(iptr *int) {
	*iptr = 0 // dereferences pointer and sets it's value
}
i := 1
zeroptr(&i)
fmt.Println("zeroptr:", i) // zeroptr: 0
```
The `&i` syntax gives the memory address of `i`, i.e. a `pointer` to `i`. 

#### Strings and Runes
A Go `string` is a read-only slice of bytes. The language and the standard library treat strings specially - as containers of text encoded in *UTF-8*. In other languages, strings are made of "characters". In Go, the concept of a character is called a `rune` - it's an integer that represents a Unicode code point.

`s` is a `string` assigned a literal value representing the word "hello" in the Thai language. Go string literals are UTF-8 encoded text:
```go
const s = "สวัสดี"
```

Since `strings` are equivalent to `[]byte`, this will produce the length of raw bytes stored within:
```go
fmt.Println("Len:", len(s)) // Len: 18
```

Indexing into a `string` produces the raw byte values at each index. This loop generates the hex values of all the bytes that constitute code points in `s`:
```go
for i := 0; i < len(s); i++ {
	fmt.Printf("%x ", s[i])
}
// e0 b8 aa e0 b8 a7 e0 b8 b1 e0 b8 aa e0 b8 94 e0 b8 b5
```

To count how many `runes` are in `string`, we can use the `utf8` package. Note that the run-time of `RuneCountInString` depends on the size of the string, because it has to decode each UTF-8 `rune` sequentially:
```go
fmt.Println("Rune count:", utf8.RuneCountInString(s))
// Rune count: 6
```

Values enclosed in single quotes are `rune` literals. We can compare a `rune` value to a `rune` literal directly:
```go
func examineRune(r rune) {
	if r == 't' {
		fmt.Println("found tee")
	} else if r == "ส" {
		fmt.Println("found so sua")
	}
}
```

##### ASCII vs Unicode vs UTF-8
- **ASCII**: A small, basic set of 128 characters. It uses 7 bits to represent characters.
- **Unicode**: A huge collection of characters from every language.
- **UTF-8**: A way to store Unicode characters efficiently. It's a variable-length encoding, meaning it can use 1 to 4 bytes to represent a character (for example, it uses 1 byte for ASCII characters, so it's compatible with ASCII).

Source: [ChatGPT](https://chatgpt.com).

##### What is a string?
In Go, a `string` is a read-only `slice` of bytes. The `string` holds *arbitrary* bytes. It is not required to hold Unicode text, UTF-8 text, or any other predefined format.

Here's a `string` literal that uses the `\xNN` notation to define a `string` constant holding some peculiar byte values:
```go
const sample = "\xbd\xb2\x3d\xbc\x20\xe2\x8c\x98"
```

##### Printing strings
Because some of the bytes in our sample `string` are not valid ASCII, not even valid UTF-8, printing the string directly will produce ugly output:
```go
fmt.Println(sample) // ��=� ⌘
```

To find out what that `string` really holds, we need to take it apart and examine the pieces. There are several ways to do this, e.g.:
```go
for i := 0; i < len(sample); i++ {
	fmt.Printf("%x ", sample[i])
}
// bd b2 3d bc 20 e2 8c 98
```
As implied up front, indexing string accesses individual bytes, not characters.

There's a shorter way to generate presentable output for a messy string: `%x` (hexadecimal) format verb of `fmt.Printf`. It just dumps out the sequential bytes of the `string` as hexadecimal digits, two per byte:
```go
fmt.Printf("%x\n", sample) // bdb23dbc20e28c98
```

A nice trick is to use the "space" flag in that format, which makes the bytes come out with spaces in-between:
```go
fmt.Printf("% x\n", sample) // bd b2 3d bc 20 e2 8c 98
```

There's more: the `%q` (quoted) verb will escape any non-printable byte sequences in a `string` so that the output is unambiguous:
```go
fmt.Printf("%q\n", sample) // "\xbd\xb2=\xbc ⌘"
```
If we squint at that, we can see ASCII equals sign, along with a regular space, and at the end appears the Swedish "Place of Interest" symbol (looped square). That symbol has Unicode value U+2318, encoded as UTF-8 by the bytes after the space (hex value `20`): `e2 8c 98`.

A "plus" flag to the `%q` verb can be used to escape not only non-printable sequences, but also any non-ASCII bytes, all while interpreting UTF-8:
```go
fmt.Printf("%+q\n", sample) // "\xbd\xb2=\xbc \u2318"
```

These printing techniques are good to know when debugging the contents of strings. They behave exactly the same for byte `slices` as they do for `strings`.

##### UTF-8 and string literals
As we saw, indexing a `string` yields its bytes, not its characters: a `string` is just a bunch of bytes. That means that when we store a character value in a `string`, we store its byte-at-a-time representation.

Here's a simple program that prints a `string` constant with a single character three different ways: once as a plain `string`, once as an ASCII-only quoted `string`, and once as individual bytes in hexadecimal. To avoid any confusion, we create a "raw string", enclosed by back quotes, so it can contain only literal text (regular strings, enclosed by double quotes, can contain escape sequences):
```go
func main() {
	const placeOfInterest = `⌘`

	fmt.Printf("plain string: ")
	fmt.Printf("%s", placeOfInterest)
	fmt.Printf("\n")
	// plain string: ⌘

	fmt.Printf("quoted string: ")
	fmt.Printf("%+q", placeOfInterest)
	fmt.Printf("\n")
	// quoted string: "\u2318"

	fmt.Printf("hex bytes: ")
	for i := 0; i < len(placeOfInterest); i++ {
		fmt.Printf("%x ", placeOfInterest[i])
	}
	fmt.Printf("\n")
	// hex bytes: e2 8c 98
}
```

It's worth taking a moment to explain how the UTF-8 representation of the `string` was created. The simple fact is: it was created when the source code was written.

Source code in Go is *defined* to be UTF-8 text; no other representation is allowed. That implies that when, in the source code, we write the text
```go
`⌘`
```
the text editor used to create the program places the UTF-8 encoding of the symbol ⌘ into the source text. When we print out the hexadecimal bytes, we're just dumping the data the editor placed in the file.

In short, Go source code is UTF-8, so *the source code for the string literal is UTF-8 text*. If that `string` literal contains no escape sequences, which a raw string cannot, the constructed `string` will hold exactly the source text between the quotes.

Go's strings are not always UTF-8: only `string` literals are. `String` *values* can contain arbitrary bytes.

To summarise, `strings` can contain arbitrary bytes, but when constructed from string literals, those bytes are (almost always, unless escaped) UTF-8.

##### Code points, characters, and runes
In Unicode, a "character" is ambiguous, so the standard uses "code point" to represent a single value. For example, U+2318 represents `⌘`, and U+0061 represents `a`. However, some characters, like `à`, can have multiple representations: U+00E0 or a combination of U+0061 (`a`) and U+0300 (combining grave accent). A character may correspond to different sequences of code points and UTF-8 bytes, so we should be cautious when using the term "character" in computing. To address the different representations, *normalisation* can be used -- it enforces a consistent form of representation.

"Code point" is a bit of a mouthful, so Go introduces a shorter term for the concept: `rune`. The term appears in the libraries and source code, and means exactly the same as "code point", with one interesting addition.

The Go language defines the word `rune` as an alias for the type `int32`, so programs can be clear when an integer value represents a code point. Moreover, what you might think of a character constant is called a `rune` constant in Go. The type and value of the expression `'⌘'` is `rune` with integer value `0x2318`.

To summarise, here are the salient points:
- Go source code is always UTF-8.
- A `string` holds arbitrary bytes.
- A `string` literal, absent byte-level escapes, always holds valid UTF-8 sequences.
- Those sequences represent Unicode code points, called `runes`.
- No guarantee is made in Go that characters in strings are normalised.

##### Range loops
Besides the axiomatic detail that Go source code is UTF-8, there's really one way that Go treats UTF-8 `strings` specially, and that is when using a `for range` loop on a `string`: it decodes one UTF-8-encoded `rune` on each iteration. The index of the loop is the starting position of the current `rune`, measured in bytes, and the code point is its value:
```go
const nihongo = "日本語"
for index, runeValue := range nihongo {
	fmt.Printf("%#U starts at byte position %d\n", runeValue, index)
}
// U+65E5 '日' starts at byte position 0
// U+672C '本' starts at byte position 3
// U+8A9E '語' starts at byte position 6
```

##### Libraries
Go's standard library provides strong support for interpreting UTF-8 text. If a `for range` loop isn't sufficient, chances are the facility needed is provided by a package in the library.

The most important such package is `unicode/utf8`, which contains helper routines to validate, disassemble, and reassemble UTF-8 strings. Here's a program equivalent to the `for range` example above, but using `DecodeRuneInString` function from that package to do the work:
```go
const nihongo = "日本語"
for i, w := 0, 0; i < len(nihongo); i += w {
	runeValue, width := utf8.DecodeRuneInString(nihongo[i:])
	fmt.Printf("%#U starts at byte position %d\n", runeValue, i)
	w = width
}
// U+65E5 '日' starts at byte position 0
// U+672C '本' starts at byte position 3
// U+8A9E '語' starts at byte position 6
```

##### Conclusion
`Strings` are built from bytes so indexing them yields bytes, not characters. A `string` might not even hold characters, and the definition of "character" is ambiguous.

Sources: [ChatGPT](https://chatgpt.com), [Go Dev Blog](https://go.dev/blog/strings).

#### Structs
Go's `structs` are typed collections of fields. They are useful for grouping data together to form records.

This `Person` struct type has `name` and `age` fields:
```go
type Person struct {
	Name string
	Age  int
}
```

`newPerson` constructs a new person `struct` with the given name:
```go
func newPerson(name string) *Person {
	p := Person{Name: name}
	p.Age = 42
	return &p
}
```
Go is a garbage collected language; you can safely return a pointer to a local variable - it will only be cleaned up by the garbage collector when there are no active references to it.
We return a `pointer` rather than `struct` itself to avoid unnecessary copying.

This syntax creates a new `struct`:
```go
fmt.Println(Person{"Bob", 20}) // {Bob 20}
```

You can name the fields when initialising a `struct`:
```go
fmt.Println(Person{Age: 30, Name: "Alice"}) // {Alice 30}
```

Omitted fields will be zero-valued:
```go
fmt.Println(Person{Name: "Fred"}) // {Fred 0}
```

An `&` prefix yields a pointer to the `struct`:
```go
fmt.Println(&Person{Name: "Ann", Age: 40}) // &{Ann 40}
```

It's idiomatic to encapsulate new `struct` creation in constructor functions:
```go
fmt.Println(newPerson("Jon")) // &{Jon 42}
```

Access struct fields with a dot:
```go
s := Person{Name: "Sean", Age: 50}
fmt.Println(s.Name) // Sean
```

You can also use dots with `struct pointers` - the `pointers` are automatically dereferenced:
```go
sp := &s
fmt.Println(sp.Age) // 50
```

`Structs` are mutable:
```go
sp.Age = 51
fmt.Println(sp.Age) // 51
```

If a `struct` type is only used for a single value, we don't have to give it a name. The value can have an anonymous `struct` type:
```go
dog := struct {
	name   string
	isGood bool
}{
	"Rex",
	true,
}
fmt.Println(dog) // {Rex true}
```
This technique is commonly used for *table-driven tests* (e.g. to define the inputs and expected outputs).

#### Methods
Go supports `methods` defined on `struct` types.

This `area` method has a *receiver* type of `*rect`:
```go
type rect struct {
	width, height int
}

func (r *rect) area() int {
	return r.width * r.height
}
```

`Methods` can be defined for either pointer or value receiver types. Here's an example of a value receiver:
```go
func (r rect) perim() int {
	return 2*r.width + 2*r.height
}
```

These `methods` are then available to be called on our `struct`:
```go
r := rect{width: 10, height: 5}
fmt.Println(r.area()) // 50
fmt.Println(r.perim()) // 30
```

Go automatically handles conversion between values and pointers for `method` calls. Pointer receiver type is used to avoid copying on method calls or mutating the receiving `struct`.
```go
rp := &r
fmt.Println(rp.area()) // 50
fmt.Println(rp.perim()) // 30
```

#### Interfaces