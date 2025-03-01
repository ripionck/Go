```markdown
# Go Interface Examples

## Examples

### 1. Basic Interface Declaration
```go
package main

import "fmt"

type Shape interface {
    Area() float64
}

type Circle struct{ Radius float64 }
func (c Circle) Area() float64 { return 3.14 * c.Radius * c.Radius }

type Rectangle struct{ Width, Height float64 }
func (r Rectangle) Area() float64 { return r.Width * r.Height }

func main() {
    var s Shape = Circle{5}
    fmt.Println("Circle Area:", s.Area()) // Output: 78.5
    
    s = Rectangle{3, 4}
    fmt.Println("Rectangle Area:", s.Area()) // Output: 12
}
```

### 2. Empty Interface (interface{})
```go
func printValue(v interface{}) {
    fmt.Printf("Type: %T, Value: %v\n", v, v)
}

// Usage:
printValue(42)      // Type: int, Value: 42
printValue("Hello") // Type: string, Value: Hello
```

### 3. Type Assertion
```go
func process(v interface{}) {
    if num, ok := v.(int); ok {
        fmt.Println("Double:", num*2) // 10 → 20
    } else {
        fmt.Println("Not an integer")
    }
}
```

### 4. Type Switch
```go
func classify(v interface{}) {
    switch v.(type) {
    case int:    fmt.Println("Integer")
    case string: fmt.Println("String")
    default:     fmt.Println("Unknown type")
    }
}
```

### 5. Error Interface
```go
type MyError struct{ Message string }
func (e MyError) Error() string { return e.Message }

func riskyOperation() error {
    return MyError{"Something went wrong!"}
}

// Usage:
err := riskyOperation()
fmt.Println(err) // Output: Something went wrong!
```

### 6. HTTP Handler Interface
```go
type HelloHandler struct{}
func (h HelloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    fmt.Fprint(w, "Hello, World!")
}

// Start server:
http.Handle("/hello", HelloHandler{})
http.ListenAndServe(":8080", nil)
```

### 7. Embedded Interfaces
```go
type ReadWriter interface {
    Reader
    Writer
}

type File struct{ Content string }
func (f *File) Read() string  { return f.Content }
func (f *File) Write(s string){ f.Content = s }

// Usage:
var rw ReadWriter = &File{}
rw.Write("Hello, Go!")
fmt.Println(rw.Read()) // Output: Hello, Go!
```

## Key Features
- **Implicit Implementation**: Types automatically satisfy interfaces
- **Polymorphism**: Interface variables can hold any implementing value
- **Type Safety**: Compile-time checks for interface satisfaction
- **Empty Interface**: Universal container for any type
- **Error Handling**: Standardized error interface pattern
- **Interface Composition**: Combine interfaces through embedding

## Core Concepts
- **Duck Typing**: "If it quacks like a duck, it's a duck"
- **Interface Values**: Store (value, type) pairs dynamically
- **Method Sets**: Determine interface compatibility
- **Type Assertions**: Runtime type checking mechanism
```
