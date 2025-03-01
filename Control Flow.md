
### Go Control Flow Explained with Examples

Go provides several control flow mechanisms for managing program execution. Here's a comprehensive guide:

---

## **1. Conditional Statements**

### a. Basic `if-else`
```go
if condition {
    // code
} else if condition2 {
    // code
} else {
    // code
}
```

**Example:**
```go
x := 15
if x > 20 {
    fmt.Println("Large")
} else if x > 10 {
    fmt.Println("Medium")  // This executes
} else {
    fmt.Println("Small")
}
```

### b. `if` with Initialization
```go
if value, err := someFunc(); err == nil {
    // Use value
} else {
    // Handle error
}
```

**Practical Use:**
```go
if file, err := os.Open("data.txt"); err != nil {
    log.Fatal(err)
} else {
    defer file.Close()
    // Process file
}
```

---

## **2. Loops**

### a. Traditional `for`
```go
for i := 0; i < 5; i++ {
    fmt.Print(i, " ")  // 0 1 2 3 4
}
```

### b. While-style
```go
count := 0
for count < 3 {
    fmt.Print(count, " ")  // 0 1 2
    count++
}
```

### c. Infinite Loop
```go
for {
    // Use break to exit
    if condition {
        break
    }
}
```

### d. Range Loops
```go
// Slice
for index, value := range []int{10, 20, 30} {
    fmt.Printf("%d: %d\n", index, value)
}

// Map
for key, value := range map[string]int{"a": 1, "b": 2} {
    fmt.Printf("%s: %d\n", key, value)
}
```

---

## **3. Switch Statements**

### a. Basic Switch
```go
switch day {
case "Mon":
    fmt.Println("Start of week")
case "Fri":
    fmt.Println("Weekend coming")
default:
    fmt.Println("Midweek")
}
```

### b. Expression Switch
```go
switch {
case num < 0:
    fmt.Println("Negative")
case num == 0:
    fmt.Println("Zero")
default:
    fmt.Println("Positive")
}
```

### c. Type Switch
```go
func checkType(v interface{}) {
    switch t := v.(type) {
    case int:
        fmt.Println("Integer:", t)
    case string:
        fmt.Println("String:", t)
    default:
        fmt.Println("Unknown type")
    }
}
```

---

## **4. Defer Mechanism**

### a. Basic Defer
```go
func main() {
    defer fmt.Println("Third")  // Executes last
    fmt.Println("First")
    fmt.Println("Second")
}
// Output: First Second Third
```

### b. LIFO Order
```go
func main() {
    defer fmt.Print("1 ")
    defer fmt.Print("2 ")
    defer fmt.Print("3 ")
}
// Output: 3 2 1
```

---

## **5. Error Handling**

### a. Panic/Recover
```go
func safeDivision(a, b int) {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered:", r)
        }
    }()
    
    if b == 0 {
        panic("division by zero")
    }
    fmt.Println(a/b)
}

// Usage:
safeDivision(10, 0)  // Recovered: division by zero
```

---

## **6. Channel Control (select)**

### a. Basic Select
```go
ch1 := make(chan string)
ch2 := make(chan string)

go func() { ch1 <- "one" }()
go func() { ch2 <- "two" }()

select {
case msg := <-ch1:
    fmt.Println(msg)
case msg := <-ch2:
    fmt.Println(msg)
}
```

### b. Timeout Pattern
```go
select {
case res := <-asyncChan:
    fmt.Println(res)
case <-time.After(2 * time.Second):
    fmt.Println("Timeout")
}
```

---

## **7. Control Keywords**

| Keyword   | Purpose                                      |
|-----------|----------------------------------------------|
| `break`   | Exit loop/switch                             |
| `continue`| Skip to next loop iteration                  |
| `goto`    | Jump to label (use sparingly)                |
| `fallthrough`| Continue to next case in switch           |

---

## **Best Practices**
1. Prefer `if` initialization for resource management
2. Use `defer` for cleanup operations
3. Avoid `goto` unless for specific error handling
4. Use `select` for non-blocking channel operations
5. Limit `panic` to truly exceptional situations
6. Keep switch cases simple and focused

---

## **Key Differences from Other Languages**
- No `while` keyword - use `for` instead
- No ternary operator (`?:`)
- Switch cases don't fall through by default
- `defer` provides deterministic cleanup
- Strict error handling via return values
