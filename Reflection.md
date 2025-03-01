
### Go Reflection (reflect package) Explained with Examples

Reflection in Go allows programs to inspect and manipulate values at runtime. The `reflect` package provides powerful but advanced functionality - use judiciously as it can bypass compile-time type safety.

## Core Concepts
- **`reflect.Type`**: Represents Go types
- **`reflect.Value`**: Holds type information and value
- **Kind**: Fundamental type category (int, struct, etc.)

## Basic Examples

### 1. Type Inspection
```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var num int = 42
    t := reflect.TypeOf(num)
    v := reflect.ValueOf(num)
    
    fmt.Println("Type:", t)         // int
    fmt.Println("Kind:", t.Kind())  // int
    fmt.Println("Value:", v.Int())  // 42
}
```

### 2. Struct Field Inspection
```go
type User struct {
    Name  string `json:"name" max:"50"`
    Age   int    `json:"age"`
    Email string `json:"email,omitempty"`
}

func inspectStruct(u User) {
    t := reflect.TypeOf(u)
    
    for i := 0; i < t.NumField(); i++ {
        field := t.Field(i)
        fmt.Printf("%s (%s) - Tags: %s\n", 
            field.Name, 
            field.Type, 
            field.Tag.Get("json"))
    }
}

// Output:
// Name (string) - Tags: name
// Age (int) - Tags: age
// Email (string) - Tags: email,omitempty
```

## Intermediate Examples

### 3. Value Modification
```go
func modifyValue() {
    var num float64 = 3.14
    
    // Get pointer value
    v := reflect.ValueOf(&num).Elem()
    
    if v.CanSet() {
        v.SetFloat(6.28)
    }
    
    fmt.Println(num) // 6.28
}
```

### 4. Dynamic Function Call
```go
func Add(a, b int) int {
    return a + b
}

func callFunction() {
    funcValue := reflect.ValueOf(Add)
    args := []reflect.Value{
        reflect.ValueOf(10),
        reflect.ValueOf(20),
    }
    
    results := funcValue.Call(args)
    fmt.Println(results[0].Int()) // 30
}
```

## Advanced Examples

### 5. Create New Instance
```go
func createInstance(t reflect.Type) interface{} {
    return reflect.New(t).Elem().Interface()
}

// Usage:
userType := reflect.TypeOf(User{})
newUser := createInstance(userType).(User)
```

### 6. Struct Tag Parsing
```go
func parseTags(s interface{}) map[string]map[string]string {
    t := reflect.TypeOf(s)
    tags := make(map[string]map[string]string)
    
    for i := 0; i < t.NumField(); i++ {
        field := t.Field(i)
        tags[field.Name] = make(map[string]string)
        
        for _, tag := range []string{"json", "max"} {
            if val := field.Tag.Get(tag); val != "" {
                tags[field.Name][tag] = val
            }
        }
    }
    
    return tags
}
```

## Common Patterns

### 7. Deep Equality Check
```go
func deepEqual(a, b interface{}) bool {
    return reflect.DeepEqual(a, b)
}

// Usage:
map1 := map[string]int{"a": 1}
map2 := map[string]int{"a": 1}
fmt.Println(deepEqual(map1, map2)) // true
```

### 8. Slice Type Conversion
```go
func convertSlice(s interface{}, targetType reflect.Type) interface{} {
    val := reflect.ValueOf(s)
    if val.Kind() != reflect.Slice {
        panic("Not a slice")
    }
    
    newSlice := reflect.MakeSlice(targetType, val.Len(), val.Cap())
    for i := 0; i < val.Len(); i++ {
        newSlice.Index(i).Set(val.Index(i).Convert(targetType.Elem()))
    }
    
    return newSlice.Interface()
}

// Usage:
intSlice := []int{1, 2, 3}
floatSlice := convertSlice(intSlice, reflect.TypeOf([]float64{})).([]float64)
```

## Important Considerations

1. **Performance**: Reflection operations are 10-100x slower than direct type access
2. **Type Safety**: No compile-time checks - potential runtime panics
3. **Readability**: Can make code harder to understand
4. **Use Cases**:
   - Serialization/Deserialization (JSON/XML)
   - ORM implementations
   - Generic programming patterns
   - Dependency injection frameworks

## Best Practices

- Prefer type assertions over reflection when possible
- Cache reflect.Type/reflect.Value in long-running operations
- Use `CanSet()`, `CanAddr()` checks before modifying values
- Wrap reflection code behind type-safe interfaces
- Always handle edge cases with `.Kind()` checks

## When to Avoid

- Performance-critical code
- Simple type conversions
- Situations where interfaces would suffice
- Code requiring high security/reliability

``` 
