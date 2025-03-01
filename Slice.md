### Go Slice Examples

### 1. **Slice Declaration & Initialization**
```go
package main

import "fmt"

func main() {
    // Declare a slice of integers (nil slice)
    var numbers []int
    fmt.Println(numbers) // Output: []

    // Initialize with a slice literal
    colors := []string{"Red", "Green", "Blue"}
    fmt.Println(colors) // Output: [Red Green Blue]

    // Using make() to create a slice with length and capacity
    nums := make([]int, 3, 5) // len=3, cap=5
    fmt.Printf("Length: %d, Capacity: %d\n", len(nums), cap(nums)) // Length: 3, Capacity: 5
}
```

---

### 2. **Appending Elements**
```go
slice := []int{1, 2, 3}
slice = append(slice, 4, 5)
fmt.Println(slice) // Output: [1 2 3 4 5]

// Append another slice
more := []int{6, 7}
slice = append(slice, more...)
fmt.Println(slice) // Output: [1 2 3 4 5 6 7]
```

---

### 3. **Slicing (Sub-slice)**
```go
original := []int{0, 1, 2, 3, 4, 5}
sub1 := original[1:3] // [1, 2]
sub2 := original[:3]  // [0, 1, 2]
sub3 := original[3:]  // [3, 4, 5]

fmt.Println(sub1, sub2, sub3)
```

---

### 4. **Iterating Over Slices**
```go
fruits := []string{"Apple", "Banana", "Cherry"}

// Using for loop
for i := 0; i < len(fruits); i++ {
    fmt.Println(fruits[i])
}

// Using range (index and value)
for index, value := range fruits {
    fmt.Printf("Index %d: %s\n", index, value)
}

// Ignore index
for _, value := range fruits {
    fmt.Println(value)
}
```

---

### 5. **Copying Slices**
```go
source := []int{1, 2, 3}
destination := make([]int, len(source))

// Copy elements
copy(destination, source)
fmt.Println(destination) // Output: [1 2 3]

// Partial copy (copies min(len(src), len(dst)) elements)
partial := make([]int, 2)
copy(partial, source)
fmt.Println(partial) // Output: [1 2]
```

---

### 6. **Multi-dimensional Slices**
```go
// 2D slice (matrix)
matrix := [][]int{
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9},
}

// Access elements
fmt.Println(matrix[1][2]) // Output: 6

// Iterate over rows and columns
for _, row := range matrix {
    for _, val := range row {
        fmt.Printf("%d ", val)
    }
    fmt.Println()
}
```

---

### 7. **Checking Slice Equality**
Slices cannot be compared with `==` directly. Use a loop or `reflect.DeepEqual`:
```go
a := []int{1, 2, 3}
b := []int{1, 2, 3}

// Manual comparison
equal := true
for i := range a {
    if a[i] != b[i] {
        equal = false
        break
    }
}
fmt.Println(equal) // Output: true

// Using reflect.DeepEqual (import "reflect")
fmt.Println(reflect.DeepEqual(a, b)) // Output: true
```

---

### 8. **Sorting Slices**
Use the `sort` package:
```go
numbers := []int{5, 2, 8, 1, 6}
sort.Ints(numbers)
fmt.Println(numbers) // Output: [1 2 5 6 8]

// Sorting strings
fruits := []string{"Cherry", "Apple", "Banana"}
sort.Strings(fruits)
fmt.Println(fruits) // Output: [Apple Banana Cherry]
```

---

### 9. **Nil vs. Empty Slices**
```go
var nilSlice []int          // nil slice
emptySlice := []int{}       // empty slice

fmt.Println(nilSlice == nil)      // true
fmt.Println(len(emptySlice) == 0) // true
```

---

### 10. **Capacity and Length**
```go
slice := make([]int, 2, 5) // len=2, cap=5
fmt.Printf("Length: %d, Capacity: %d\n", len(slice), cap(slice)) // Length: 2, Capacity: 5

// Appending beyond capacity triggers a new allocation
slice = append(slice, 3, 4, 5, 6)
fmt.Printf("New Length: %d, New Capacity: %d\n", len(slice), cap(slice)) // New Capacity: 10
```

---

### Key Points:
- **Dynamic Size**: Slices grow/shrink dynamically (unlike arrays).
- **Reference Type**: Slices reference an underlying array.
- **Capacity**: The capacity of a slice doubles when it exceeds during `append()`.
- **Use Cases**: Ideal for dynamic collections (e.g., lists, buffers).
- **Best Practices**:
  - Use `make` to pre-allocate slices for performance.
  - Prefer slices over arrays for most use cases.
  - Use `copy()` to duplicate slices safely.
