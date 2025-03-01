
## Functions in Go: Explanation with Examples

Go functions are first-class citizens and support powerful features like multiple return values, named returns, and closures.

---

## **1. Basic Function**

**Syntax:**
```go
func name(parameters) return-type {
    // code
}
```

**Example:**
```go
func add(a int, b int) int {
    return a + b
}

// Usage:
sum := add(3, 5)  // 8
```

---

## **2. Multiple Return Values**

**Common for error handling:**
```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Usage:
result, err := divide(10, 2)
if err != nil {
    log.Fatal(err)
}
```

---

## **3. Named Return Values**

**Document return values and enable "naked" returns:**
```go
func rectangleProps(length, width float64) (area, perimeter float64) {
    area = length * width
    perimeter = 2 * (length + width)
    return  // Naked return
}

// Usage:
a, p := rectangleProps(5, 3)  // a=15, p=16
```

---

## **4. Variadic Functions**

**Accept variable number of arguments:**
```go
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

// Usage:
s1 := sum(1, 2, 3)    // 6
s2 := sum([]int{4,5}...) // Unpack slice
```

---

## **5. Defer Statements**

**Schedule execution after function completes:**
```go
func readFile(filename string) {
    file, err := os.Open(filename)
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()  // Executes last
    
    // Process file
}
```

---

## **6. Anonymous Functions & Closures**

**Function literals and closure example:**
```go
func main() {
    // Anonymous function
    greet := func(name string) {
        fmt.Println("Hello", name)
    }
    greet("Alice")  // Hello Alice

    // Closure
    counter := func() func() int {
        count := 0
        return func() int {
            count++
            return count
        }
    }()
    
    fmt.Println(counter())  // 1
    fmt.Println(counter())  // 2
}
```

---

## **7. Recursion**

**Factorial example:**
```go
func factorial(n int) int {
    if n == 0 {
        return 1
    }
    return n * factorial(n-1)
}

// Usage:
fmt.Println(factorial(5))  // 120
```

---

## **Key Features**
- **First-class functions**: Can be assigned to variables
- **No overloading**: Functions must have unique names
- **Pass by value**: Default parameter passing
- **Function types**:
  ```go
  type Operation func(int, int) int
  var compute Operation = add
  ```

---

## **Best Practices**
1. Keep functions small and focused (single responsibility)
2. Use multiple return values for error handling
3. Name return values for documentation
4. Use defer for resource cleanup
5. Avoid naked returns in long functions
6. Prefer explicit error handling over panic/recover

---

## **Common Patterns**
**1. Error Wrapping:**
```go
func loadConfig() (Config, error) {
    data, err := os.ReadFile("config.json")
    if err != nil {
        return Config{}, fmt.Errorf("config error: %w", err)
    }
    // ...
}
```

**2. Middleware Pattern:**
```go
func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        log.Println(r.URL.Path)
        next.ServeHTTP(w, r)
    })
}
```

**3. Defer for Timing:**
```go
func trackTime(start time.Time, name string) {
    elapsed := time.Since(start)
    log.Printf("%s took %s", name, elapsed)
}

func process() {
    defer trackTime(time.Now(), "process")
    // ...
}
```
