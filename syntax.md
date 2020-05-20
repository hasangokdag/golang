# Syntax

## variables

Variables can be declared both in package or function level.

```go
var x, y, z int
var year, month int = 1997, 4 //same with var year, month = 1997, 4
var t, f, str = true, false , "string"
variable := 5 // := is short assignment and only works within function
var (
    x int    // 0
    y string // ""
    z bool   // false
)
```

`const` exists but cant be used with `:=`
defaults for int, bool and string are `0 , false, ""`

## main structure

```go
package main

import (
    "fmt"   // I/O package
    "math"  // Math package
)

func main () {
    fmt.Println(math.Pi)
}
```

Only exported names of packages can be used. Make the first letter capital to export a name from package.

- math.Pi✔️
- math.pi✖️

## functions

```go
func foo(x int, y int) int { // or shortly (x, y int) if both are the same type
    return x+y
}
```

```go
func swap(x, y string) (string, string){
    return y, x
}
```

no need for `return x,y` if

```go
func foo(input int) (a, b int) {
    a = input+5
    b = 10-input
    return
}
```

functions can be values of other functions:

```go
func compute(fn func(int,int) int) int {
  return fn(3,4)
}

compute(math.Pow)   // 3^4 = 81
compute(math.Hypot) // Sqrt(3^2+4^2) = 5
```

### closures

//TODO
[closures on golang tour](https://tour.golang.org/moretypes/25)

### for

```go
for i := 0; i < 10; i++ { //init;condition;post
  sum += i
 }
```

```go
sum := 50
for ; sum < 1000; { //init and post components are optional
  sum += sum
 }
```

using slices with `range`:

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
for i, v := range pow {  // slices return index and value when used with range
  fmt.Printf("2**%d = %d\n", i, v)
 }
```

assign `_` to skip index or value:

```go
for i, _ := range pow // same as for i := range pow
for _, value := range pow
```

### while

`for` is also used as while in go

```go
 sum := 1
 for sum < 1000 { //no semicolons
  sum += sum
 }
```

### forever

```go

for {
    fmt.Println ("hi")
}
```

### if

```go

if x > 10 {
    //do smth
}
else {
    //do smth else
}
```

if can have a short statement before condition

```go
 if v := math.Pow(x, n); v < lim {
  return v
}
```

### switch

cases are evaluated from top to bottom. when a case succeeds, it breaks.

```go
switch os := runtime.GOOS; os {
 case "darwin":
  fmt.Println("OS X.")
 case "linux":
  fmt.Println("Linux.")
 default:
    fmt.Printf("%s.\n", os)
    }
```

switches without consitions are if-else statements.

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

### defer

A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in *last-in-first-out* order.

```go
func main() {
 defer fmt.Println("world")

 fmt.Println("hello")
} //prints hello world
```

### pointers

`*t` is a pointer to a t value. its zero value is `nil`.

```go

i := 42
p := &i         // point to i
fmt.Println(*p) // read i through the pointer
*p = 21         // set i through the pointer
fmt.Println(i)  // see the new value of i ->21
```

---

## data structures

### structs

struct fields are accessed through dot.

```go
type Vertex struct {
 X int
 Y int
}

func main() {
 v := Vertex{1, 2}
 v.X = 4
 fmt.Println(v.X) // -> 4
}

```

pointers can also be used

```go
 v := Vertex{1, 2}
 p := &v
 p.X = 1e9
```

```go
var (
 v1 = Vertex{1, 2}  // has type Vertex
 v2 = Vertex{X: 1}  // Y:0 is implicit
 v3 = Vertex{}      // X:0 and Y:0
 p  = &Vertex{1, 2} // has type *Vertex
)
```

### arrays

arrays can't be resized (can be solved using `make`)

```go
var a [2]string // array of 2 strings
a[0] = "Hello"
a[1] = "World"
fmt.Println(a) // [Hello World]
fmt.Println(a[1]) // World

primes := [6]int{2, 3, 5, 7, 11, 13}
```

### slices

more common than arrays.
does not store elements, just displays them. any change to slice is reflected to original array.
`len(s)` length displays size of the slice
`cap(s)` capacity displays size of underlying array
zero value: `nil`

low is included high is not included

```go
a[low : high]
a[:10] // start to 10
a[0:]  // 0 to end
a[:]   // start to end
```

```go
var s []int = primes[1:4] //elements 1,2,3
```

```go
[]bool{true, true, false} //slice literal
```

```go
a := make([]int, 5)     // len=5 cap=5 [0 0 0 0 0]
b := make([]int, 0, 5)  // len=0 cap=5 []
```

slices can be appended
underlying array is reallocated when it's full

```go
var s []int

s = append(s,1,2,3)
```

### maps

key:value
zero value : `nil`
`make` returns initialized map of given type

```go
type Vertex struct {
 Lat, Long float64
}

var m map[string]Vertex // create the map

func main() {
 m = make(map[string]Vertex) //initialize the map
 m["Bell Labs"] = Vertex{
  40.68433, -74.39967,
 }
 fmt.Println(m["Bell Labs"])
}

```

map literals:

```go
var m = map[string]Vertex{
 "Bell Labs": {40.68433, -74.39967,},
 "Google": {37.42202, -122.08408,},
}
```

```go
m["key"] = value //insert

value = m["key"] //retrieve

delete(m,"key")  //delete

elem, ok = m["key"] //retrieve and check
//elem=0     ok=false     if key is not present
//elem=value ok=true      if key is present
