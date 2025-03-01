### Go Map Examples

---
### 1. **Map Declaration & Initialization**
```go
package main

import "fmt"

func main() {
    // Declare a nil map (not initialized)
    var m1 map[string]int
    fmt.Println(m1) // Output: map[]

    // Initialize with a map literal
    m2 := map[string]int{
        "Alice": 30,
        "Bob":   25,
    }
    fmt.Println(m2) // Output: map[Alice:30 Bob:25]

    // Using make() to create a map
    m3 := make(map[string]string)
    m3["name"] = "Charlie"
    fmt.Println(m3) // Output: map[name:Charlie]
}
```

---

### 2. **Adding/Updating Elements**
```go
m := make(map[string]int)

// Add key-value pairs
m["apple"] = 1
m["banana"] = 2

// Update value
m["apple"] = 10
fmt.Println(m) // Output: map[apple:10 banana:2]
```

---

### 3. **Deleting Elements**
```go
m := map[string]int{"a": 1, "b": 2}
delete(m, "a")
fmt.Println(m) // Output: map[b:2]

// Deleting a non-existent key is safe (no error)
delete(m, "unknown_key")
```

---

### 4. **Checking Key Existence**
```go
m := map[string]int{"Alice": 30}

// Comma-ok idiom to check existence
age, exists := m["Alice"]
if exists {
    fmt.Printf("Age: %d\n", age) // Output: Age: 30
}

// Check without using the value
_, found := m["Bob"]
fmt.Println(found) // Output: false
```

---

### 5. **Iterating Over Maps**
```go
m := map[string]int{"a": 1, "b": 2, "c": 3}

// Iterate with range (order is not guaranteed)
for key, value := range m {
    fmt.Printf("Key: %s, Value: %d\n", key, value)
}

// Iterate keys only
for key := range m {
    fmt.Println("Key:", key)
}

// Iterate values only
for _, value := range m {
    fmt.Println("Value:", value)
}
```

---

### 6. **Map with Struct Values**
```go
type Person struct {
    Name string
    Age  int
}

func main() {
    people := map[string]Person{
        "Alice": {"Alice", 30},
        "Bob":   {"Bob", 25},
    }

    fmt.Println(people["Alice"]) // Output: {Alice 30}
}
```

---

### 7. **Nil Maps**
A nil map cannot be modified (causes panic):
```go
var m map[string]int
fmt.Println(m == nil) // true

// m["key"] = 42 // This would panic!
```

---

### 8. **Merging Maps**
```go
m1 := map[string]int{"a": 1, "b": 2}
m2 := map[string]int{"c": 3, "d": 4}

// Merge m2 into m1
for k, v := range m2 {
    m1[k] = v
}
fmt.Println(m1) // Output: map[a:1 b:2 c:3 d:4]
```

---

### 9. **Sorting Map Keys**
Maps are unordered, but you can sort keys for iteration:
```go
m := map[string]int{"banana": 2, "apple": 1, "cherry": 3}

// Extract and sort keys
keys := make([]string, 0, len(m))
for k := range m {
    keys = append(keys, k)
}
sort.Strings(keys)

// Iterate in sorted order
for _, k := range keys {
    fmt.Printf("%s: %d\n", k, m[k])
}
// Output:
// apple: 1
// banana: 2
// cherry: 3
```

---

### Key Points:
- **Reference Type**: Maps are references to hash tables.
- **Concurrency**: Maps are **not thread-safe** (use `sync.Map` or mutexes for concurrent access).
- **Nil Maps**: Cannot be modified (initialize with `make` or literals).
- **Order**: Map iteration order is random (do not rely on it).
- **Performance**: Efficient for lookups, inserts, and deletes (average O(1)).
