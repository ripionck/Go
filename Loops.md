### Loops in Go
Go has only one looping construct: the `for` loop. It can be used in 4 different ways:

### 1. Basic For Loop
```go
for initialization; condition; post {
    // loop body
}
```

**Example:**
```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
// Output: 0 1 2 3 4
```

---

### 2. While-like Loop
```go
for condition {
    // loop body
}
```

**Example:**
```go
count := 0
for count < 3 {
    fmt.Println(count)
    count++
}
// Output: 0 1 2
```

---

### 3. Infinite Loop
```go
for {
    // loop body (use break to exit)
}
```

**Example:**
```go
num := 0
for {
    if num >= 3 {
        break
    }
    fmt.Println(num)
    num++
}
// Output: 0 1 2
```

---

### 4. Range Loop (For-Each)
```go
for index, value := range collection {
    // loop body
}
```

**Examples:**
```go
// Slice iteration
fruits := []string{"Apple", "Banana", "Cherry"}
for i, fruit := range fruits {
    fmt.Printf("%d: %s\n", i, fruit)
}
// Output:
// 0: Apple
// 1: Banana
// 2: Cherry

// Map iteration
ages := map[string]int{
    "Alice": 25,
    "Bob":   30,
}
for name, age := range ages {
    fmt.Printf("%s is %d\n", name, age)
}

// String iteration (Unicode code points)
for i, r := range "Hello" {
    fmt.Printf("%d: %c\n", i, r)
}
```

---

## Loop Control Statements
1. **`break`** - Exit loop immediately
   ```go
   for i := 0; i < 10; i++ {
       if i == 5 {
           break
       }
       fmt.Println(i)
   }
   // Output: 0 1 2 3 4
   ```

2. **`continue`** - Skip to next iteration
   ```go
   for i := 0; i < 5; i++ {
       if i == 2 {
           continue
       }
       fmt.Println(i)
   }
   // Output: 0 1 3 4
   ```

---

## Nested Loops
```go
for i := 0; i < 3; i++ {
    for j := 0; j < 2; j++ {
        fmt.Printf("%d-%d ", i, j)
    }
    fmt.Println()
}
// Output:
// 0-0 0-1 
// 1-0 1-1 
// 2-0 2-1
```

---

## Key Features
- **No parentheses**: Unlike C-family languages
  ```go
  // Correct
  for i := 0; i < 5; i++ {
  }

  // Incorrect
  for (i := 0; i < 5; i++) {
  }
  ```
- **Single looping construct**: `for` replaces while/do-while
- **Range flexibility**: Works with arrays, slices, maps, strings, channels
- **Blank identifier**: Ignore values in range loops
  ```go
  for _, value := range []int{1,2,3} {
      fmt.Println(value)
  }
  ```

---

## Common Patterns
1. **Slice filtering**
   ```go
   numbers := []int{1, 2, 3, 4, 5}
   var even []int
   for _, n := range numbers {
       if n%2 == 0 {
           even = append(even, n)
       }
   }
   ```

2. **Infinite server loop**
   ```go
   for {
       conn, err := listener.Accept()
       if err != nil {
           log.Fatal(err)
       }
       go handleConnection(conn)
   }
   ```

3. **Timeout loop**
   ```go
   timeout := time.After(5 * time.Second)
   for {
       select {
       case <-timeout:
           fmt.Println("Timed out!")
           return
       default:
           // Do work
       }
   }
   ```

---

## Best Practices
1. Use range loops for collection iteration
2. Avoid infinite loops without exit conditions
3. Use meaningful loop variable names
4. Prefer `for` over `goto` for flow control
5. Use `defer` carefully inside loops (might accumulate resources)
