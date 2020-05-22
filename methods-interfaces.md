# Methods and Interfaces

---

This document contains the notes from the "Methods and Interfaces" part of [Tour of Go](https://tour.golang.org/list).

## Table of Contents

1. [methods](#methods)

---

## methods

Go doesn't have classes. There are methods on types.
A method is a function with a special receiver argument.

The receiver appears in its own argument list between the func keyword and the method name.

```go
type Vertex struct {
 X, Y float64
}

func (v Vertex) Abs() float64 {     // Vertex type receiver named v
 return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
 v := Vertex{3, 4}
 fmt.Println(v.Abs()) // v.Abs()
}
```

Same functionality can be achieved with

```go
type Vertex struct {
 X, Y float64
}

func Abs(v Vertex) float64 {
 return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
 v := Vertex{3, 4}
 fmt.Println(Abs(v)) // Abs(v)
}
```

Methods can also be declared for non-struct types.

```go
type MyFloat float64

func (f MyFloat) Abs() float64 {}
```
