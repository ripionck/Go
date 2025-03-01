### Go Generics Examples

### 1. Basic Generic Function
```go
package main

import "fmt"

// Min returns the smaller of two values
func Min[T constraints.Ordered](a, b T) T {
    if a < b {
        return a
    }
    return b
}

func main() {
    fmt.Println(Min(3, 5))       // Output: 3
    fmt.Println(Min(2.7, 1.5))   // Output: 1.5
    fmt.Println(Min("a", "c"))   // Output: a
}
```

### 2. Generic Struct
```go
// Box can hold any type
type Box[T any] struct {
    Content T
}

func (b Box[T]) Unwrap() T {
    return b.Content
}

// Usage:
intBox := Box[int]{Content: 42}
strBox := Box[string]{Content: "Hello"}
fmt.Println(intBox.Unwrap()) // 42
fmt.Println(strBox.Unwrap()) // Hello
```

### 3. Constraints with Interface
```go
import "golang.org/x/exp/constraints"

// Sum numbers of any numeric type
func Sum[T constraints.Integer | constraints.Float](nums []T) T {
    var total T
    for _, n := range nums {
        total += n
    }
    return total
}

// Usage:
fmt.Println(Sum([]int{1,2,3}))        // 6
fmt.Println(Sum([]float64{1.1, 2.2})) // 3.3
```

### 4. Type-Safe Map Function
```go
// Map applies function to slice elements
func Map[T, U any](slice []T, f func(T) U) []U {
    result := make([]U, len(slice))
    for i, v := range slice {
        result[i] = f(v)
    }
    return result
}

// Usage:
numbers := []int{1, 2, 3}
doubled := Map(numbers, func(n int) int { return n*2 })
fmt.Println(doubled) // [2 4 6]
```

### 5. Custom Constraint
```go
// Stringer constraint for types with String() method
type Stringer interface {
    String() string
}

func PrintString[T Stringer](value T) {
    fmt.Println(value.String())
}

// Implement Stringer
type ID int

func (i ID) String() string {
    return fmt.Sprintf("ID-%d", i)
}

// Usage:
PrintString(ID(42)) // Output: ID-42
```

### 6. Generic Cache
```go
type Cache[K comparable, V any] struct {
    store map[K]V
    mu    sync.Mutex
}

func NewCache[K comparable, V any]() *Cache[K, V] {
    return &Cache[K, V]{store: make(map[K]V)}
}

func (c *Cache[K, V]) Set(key K, value V) {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.store[key] = value
}

func (c *Cache[K, V]) Get(key K) (V, bool) {
    c.mu.Lock()
    defer c.mu.Unlock()
    val, ok := c.store[key]
    return val, ok
}
```

### 7. Generic Result Type
```go
type Result[T any] struct {
    Value T
    Err   error
}

func (r Result[T]) Match(success func(T), failure func(error)) {
    if r.Err != nil {
        failure(r.Err)
    } else {
        success(r.Value)
    }
}

// Usage:
res := Result[int]{Value: 42}
res.Match(
    func(v int) { fmt.Println("Success:", v) },
    func(e error) { fmt.Println("Error:", e) },
)
// Output: Success: 42
```

## Key Features
- **Type Parameters**: Declare with `[T constraint]` syntax
- **Constraints**: Use built-in (`any`, `comparable`) or custom constraints
- **Type Inference**: Compiler often deduces types automatically
- **Generic Data Structures**: Create reusable containers
- **Method Support**: Generics in methods and struct fields
- **Performance**: Generics generate specialized code at compile time

## When to Use Generics
- Avoid code duplication for multiple types
- Create type-safe containers
- Implement generic algorithms
- Work with collection types

## Limitations
- No method-only generics (methods cannot have new type parameters)
- Limited operator support (need constraints for operations)
- Increased binary size for multiple type instantiations
```
