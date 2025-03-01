
### Go File Handling Examples

```markdown
### 1. Basic File Operations
```go
package main

import (
    "fmt"
    "os"
)

func main() {
    // Write file
    content := []byte("Hello, Go File System!")
    err := os.WriteFile("example.txt", content, 0644)
    if err != nil {
        panic(err)
    }

    // Read file
    data, err := os.ReadFile("example.txt")
    if err != nil {
        panic(err)
    }
    fmt.Println(string(data)) // Hello, Go File System!

    // Append to file
    f, err := os.OpenFile("example.txt", os.O_APPEND|os.O_WRONLY, 0644)
    if err != nil {
        panic(err)
    }
    defer f.Close()
    
    if _, err = f.WriteString("\nAppended line"); err != nil {
        panic(err)
    }
}
```

### 2. CSV File Handling
```go
import (
    "encoding/csv"
    "strings"
)

func handleCSV() {
    // Write CSV
    csvData := [][]string{
        {"Name", "Age", "City"},
        {"Alice", "30", "New York"},
        {"Bob", "25", "London"},
    }
    
    file, _ := os.Create("data.csv")
    defer file.Close()
    
    writer := csv.NewWriter(file)
    defer writer.Flush()
    
    for _, row := range csvData {
        _ = writer.Write(row)
    }

    // Read CSV
    file, _ := os.Open("data.csv")
    reader := csv.NewReader(file)
    records, _ := reader.ReadAll()
    
    for _, record := range records {
        fmt.Println(record)
    }
}
```

### 3. JSON File Handling
```go
type Person struct {
    Name  string `json:"name"`
    Age   int    `json:"age"`
    Email string `json:"email"`
}

func handleJSON() {
    // Write JSON
    person := Person{"Alice", 30, "alice@example.com"}
    data, _ := json.MarshalIndent(person, "", "  ")
    _ = os.WriteFile("person.json", data, 0644)

    // Read JSON
    file, _ := os.ReadFile("person.json")
    var decodedPerson Person
    _ = json.Unmarshal(file, &decodedPerson)
    fmt.Printf("%+v\n", decodedPerson)
}
```

### 4. XML File Handling
```go
type Book struct {
    XMLName xml.Name `xml:"book"`
    Title   string   `xml:"title"`
    Author  string   `xml:"author"`
    Year    int      `xml:"year,attr"`
}

func handleXML() {
    // Write XML
    book := Book{Title: "The Go Programming Language", Author: "Donovan & Kernighan", Year: 2015}
    data, _ := xml.MarshalIndent(book, "", "  ")
    _ = os.WriteFile("book.xml", data, 0644)

    // Read XML
    file, _ := os.ReadFile("book.xml")
    var decodedBook Book
    _ = xml.Unmarshal(file, &decodedBook)
    fmt.Printf("%+v\n", decodedBook)
}
```

### 5. Error Handling Patterns
```go
func safeFileOperation() {
    // Check if file exists
    if _, err := os.Stat("missing.txt"); os.IsNotExist(err) {
        fmt.Println("File doesn't exist")
    }

    // Graceful error handling
    file, err := os.Open("nonexistent.txt")
    if err != nil {
        if pathErr, ok := err.(*os.PathError); ok {
            fmt.Printf("Path error: %v\n", pathErr)
        } else {
            panic(err)
        }
    }
    defer file.Close()
}

func createDirIfNotExist(path string) {
    if _, err := os.Stat(path); os.IsNotExist(err) {
        os.MkdirAll(path, 0755)
    }
}
```

### 6. Buffered File Operations
```go
func bufferedIO() {
    // Buffered writing
    file, _ := os.Create("buffered.txt")
    defer file.Close()
    
    writer := bufio.NewWriter(file)
    writer.WriteString("First line\n")
    writer.WriteString("Second line\n")
    writer.Flush()

    // Buffered reading
    file, _ := os.Open("buffered.txt")
    scanner := bufio.NewScanner(file)
    for scanner.Scan() {
        fmt.Println(scanner.Text())
    }
}
```

## Key Concepts
- **File Modes**: `0644` (read/write permissions)
- **Common Packages**:
  - `os`: Basic file operations
  - `io/ioutil`: Higher-level I/O functions (deprecated in Go 1.16+)
  - `bufio`: Buffered I/O
  - `encoding/json`, `encoding/xml`, `encoding/csv`: Data formats
- **Error Types**:
  - `*os.PathError`: Path-related errors
  - `*os.LinkError`: File linking errors
  - `*os.SyscallError`: System call errors
- **Best Practices**:
  - Always check errors
  - Use `defer` for closing files
  - Prefer `os.ReadFile`/`os.WriteFile` for small files
  - Use buffered I/O for large files
  - Validate file paths and permissions

## Common Operations Cheat Sheet
| Operation               | Method                          |
|-------------------------|---------------------------------|
| Create file             | `os.Create()`                   |
| Open file               | `os.Open()`                     |
| Read entire file        | `os.ReadFile()`                 |
| Write entire file       | `os.WriteFile()`                |
| Check file existence    | `os.Stat()` + `os.IsNotExist()` |
| Create directory        | `os.MkdirAll()`                 |
| Remove file             | `os.Remove()`                   |


## Important Notes
- Always handle errors explicitly in production code
- Be cautious with file permissions
- Use `path/filepath` for cross-platform path handling
- Consider file locking for concurrent access
- Clean up temporary files after use
```
