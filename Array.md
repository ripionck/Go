### Go Array Examples

### 1. **Array Declaration & Initialization**
```go
package main

import "fmt"

func main() {
    // Declare an array of 3 integers
    var numbers [3]int
    numbers[0] = 10
    numbers[1] = 20
    numbers[2] = 30
    fmt.Println(numbers) // Output: [10 20 30]

    // Initialize with array literal
    colors := [3]string{"Red", "Green", "Blue"}
    fmt.Println(colors) // Output: [Red Green Blue]

    // Let Go infer the length with ...
    fruits := [...]string{"Apple", "Banana", "Cherry"}
    fmt.Println(fruits) // Output: [Apple Banana Cherry]
}
```

---

### 2. **Accessing Elements**
```go
arr := [5]int{10, 20, 30, 40, 50}
fmt.Println(arr[0])  // Output: 10
fmt.Println(arr[2])  // Output: 30

// Modify an element
arr[3] = 400
fmt.Println(arr)     // Output: [10 20 30 400 50]
```

---

### 3. **Iterating Over Arrays**
```go
// Using a for loop
for i := 0; i < len(arr); i++ {
    fmt.Printf("Index %d: %d\n", i, arr[i])
}

// Using range (index and value)
for index, value := range arr {
    fmt.Printf("Index %d: %d\n", index, value)
}

// Ignore index (use blank identifier)
for _, value := range arr {
    fmt.Println(value)
}
```

---

### 4. **Multi-dimensional Arrays**
```go
// 2D array (3x2 matrix)
matrix := [3][2]int{
    {1, 2},
    {3, 4},
    {5, 6},
}

// Access elements
fmt.Println(matrix[0][1]) // Output: 2

// Iterate over 2D array
for _, row := range matrix {
    for _, val := range row {
        fmt.Printf("%d ", val)
    }
    fmt.Println()
}
// Output:
// 1 2 
// 3 4 
// 5 6 
```

---

### 5. **Array Comparison**
Arrays are **comparable** if their elements are comparable:
```go
a := [2]int{1, 2}
b := [2]int{1, 2}
c := [2]int{3, 4}

fmt.Println(a == b) // true
fmt.Println(a == c) // false
```

---

### 6. **Passing Arrays to Functions**
Arrays are **copied** when passed to functions (value semantics):
```go
func printArray(arr [3]int) {
    fmt.Println(arr)
}

func main() {
    nums := [3]int{1, 2, 3}
    printArray(nums) // Output: [1 2 3]
}
```

---

### 7. **Array Length**
Use `len()` to get the array size:
```go
arr := [5]int{}
fmt.Println(len(arr)) // Output: 5
```

---

### 8. **Copying Arrays**
Arrays are copied by value:
```go
a := [3]int{1, 2, 3}
b := a // Full copy
b[0] = 100
fmt.Println(a) // Output: [1 2 3]
fmt.Println(b) // Output: [100 2 3]
```

---

### Key Points:
- **Fixed Size**: Array length is part of the type (e.g., `[3]int` vs `[4]int`).
- **Value Semantics**: Arrays are copied when assigned or passed to functions.
- **Use Cases**: Useful for fixed-size data (e.g., matrices, buffers).
- **Limitations**: Prefer **slices** for dynamic sizes and flexibility.
