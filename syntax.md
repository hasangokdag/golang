# Syntax

## variables

Variables can be declared both in package or function level.

```go
var x, y, z int
var year, month int = 1997, 4 //same with var year, month = 1997, 4
var t, f, str = true, false , "string"
variable := 5 // := is short assignment and only works within function
```

`const` exists and cant be used with `:=`
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

### while

`for` is also used as while in go

```go
 sum := 1
 for sum < 1000 { //no semicolons
  sum += sum
 }
```

###forever

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