### Go Concurrency Examples

### 1. Basic Goroutine
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    go func() {
        fmt.Println("New goroutine running")
    }()
    
    time.Sleep(100 * time.Millisecond) // Wait for goroutine
    fmt.Println("Main goroutine done")
}
// Output (order may vary):
// New goroutine running
// Main goroutine done
```

### 2. WaitGroup for Synchronization
```go
func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d starting\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {
    var wg sync.WaitGroup
    
    for i := 1; i <= 3; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }
    
    wg.Wait()
    fmt.Println("All workers completed")
}
```

### 3. Channels (Unbuffered)
```go
func main() {
    ch := make(chan string)
    
    go func() {
        ch <- "Hello from goroutine!"
    }()
    
    msg := <-ch
    fmt.Println(msg) // Hello from goroutine!
}
```

### 4. Buffered Channels
```go
func main() {
    ch := make(chan int, 2)
    
    ch <- 1
    ch <- 2
    // ch <- 3  // Would block (buffer full)
    
    fmt.Println(<-ch) // 1
    fmt.Println(<-ch) // 2
}
```

### 5. Select Statement
```go
func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    
    go func() { ch1 <- "one" }()
    go func() { ch2 <- "two" }()
    
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        }
    }
}
// Possible Output:
// one
// two
```

### 6. Mutex for Shared State
```go
type SafeCounter struct {
    mu    sync.Mutex
    value int
}

func (c *SafeCounter) Inc() {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.value++
}

func main() {
    counter := SafeCounter{}
    
    for i := 0; i < 1000; i++ {
        go counter.Inc()
    }
    
    time.Sleep(time.Second)
    fmt.Println(counter.value) // 1000
}
```

### 7. Worker Pool Pattern
```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Printf("Worker %d started job %d\n", id, j)
        time.Sleep(time.Second)
        results <- j * 2
        fmt.Printf("Worker %d finished job %d\n", id, j)
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)
    
    // Start 3 workers
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }
    
    // Send 5 jobs
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)
    
    // Collect results
    for a := 1; a <= 5; a++ {
        <-results
    }
}
```

### 8. Context Cancellation
```go
func longProcess(ctx context.Context) {
    select {
    case <-time.After(5 * time.Second):
        fmt.Println("Process completed")
    case <-ctx.Done():
        fmt.Println("Process canceled:", ctx.Err())
    }
}

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()
    
    go longProcess(ctx)
    time.Sleep(3 * time.Second) // Wait for cancellation
}
// Output: Process canceled: context deadline exceeded
```

### 9. Error Handling in Goroutines
```go
func fetchURL(url string, errCh chan<- error) {
    // Simulate HTTP request
    if rand.Intn(2) == 0 {
        errCh <- fmt.Errorf("failed to fetch %s", url)
        return
    }
    fmt.Printf("Success: %s\n", url)
}

func main() {
    urls := []string{"a.com", "b.com", "c.com"}
    errCh := make(chan error)
    
    for _, url := range urls {
        go fetchURL(url, errCh)
    }
    
    for range urls {
        if err := <-errCh; err != nil {
            fmt.Println("Error:", err)
        }
    }
}
```

### 10. Fan-out/Fan-in Pattern
```go
func producer(nums ...int) <-chan int {
    out := make(chan int)
    go func() {
        for _, n := range nums {
            out <- n
        }
        close(out)
    }()
    return out
}

func square(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    return out
}

func main() {
    // Set up pipeline: producer -> square
    in := producer(1, 2, 3)
    out := square(in)
    
    for n := range out {
        fmt.Println(n) // 1, 4, 9
    }
}
```

## Key Concepts
- **Goroutines**: Lightweight threads managed by Go runtime
- **Channels**: Communication mechanism between goroutines
- **Select**: Handle multiple channel operations
- **Sync Primitives**: `WaitGroup`, `Mutex`, `RWMutex`
- **Patterns**:
  - Worker Pool
  - Fan-out/Fan-in
  - Pipeline
  - Context cancellation
- **Best Practices**:
  - Use `select` with `context.Done()`
  - Prefer channels over shared memory
  - Always clean up goroutines
  - Use `sync.Once` for singleton initialization

## Common Pitfalls
- Goroutine leaks (unterminated goroutines)
- Deadlocks from improper channel synchronization
- Race conditions (use `go run -race` to detect)
- Unbuffered channel blocking
