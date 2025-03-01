### Go Struct Examples

### 1. Basic Struct Declaration
```go
package main

import "fmt"

type Person struct {
    Name    string
    Age     int
    Address string
}

func main() {
    var p Person
    p.Name = "Alice"
    p.Age = 30
    p.Address = "123 Main St"
    fmt.Println(p) // Output: {Alice 30 123 Main St}
}
```

### 2. Struct Initialization with Literals
```go
p := Person{
    Name:    "Bob",
    Age:     25,
    Address: "456 Oak Ave",
}
fmt.Printf("%+v\n", p) // Output: {Name:Bob Age:25 Address:456 Oak Ave}
```

### 3. Structs with Methods
```go
// Value receiver method
func (p Person) IsAdult() bool {
    return p.Age >= 18
}

fmt.Println(p.IsAdult()) // Output: true
```

### 4. Nested Structs
```go
type Address struct {
    Street string
    City   string
    Zip    string
}

type Employee struct {
    Name    string
    Age     int
    Address Address
}

e := Employee{
    Name: "Charlie",
    Age:  40,
    Address: Address{
        Street: "789 Pine Rd",
        City:   "Tech City",
        Zip:    "12345",
    },
}
```

### 5. Struct Tags for JSON
```go
type User struct {
    Username string `json:"username"`
    Email    string `json:"email"`
    isAdmin  bool   `json:"-"` // Unexported field (ignored in JSON)
}

u := User{"admin", "admin@example.com", true}
data, _ := json.Marshal(u)
fmt.Println(string(data)) // Output: {"username":"admin","email":"admin@example.com"}
```

### 6. Anonymous Structs
```go
book := struct {
    Title  string
    Author string
    Pages  int
}{
    Title:  "The Go Programming Language",
    Author: "Alan A. A. Donovan",
    Pages:  380,
}
fmt.Println(book.Title) // Output: The Go Programming Language
```

### 7. Struct Embedding (Composition)
```go
type Animal struct {
    Name string
}

func (a Animal) Speak() string {
    return "Some generic sound"
}

type Dog struct {
    Animal // Embedded struct (promotes fields/methods)
    Breed  string
}

d := Dog{
    Animal: Animal{Name: "Rex"},
    Breed:  "Golden Retriever",
}
fmt.Println(d.Speak()) // Output: Some generic sound
fmt.Println(d.Name)    // Output: Rex
```

## Key Features
- **Value Semantics**: Structs are copied when assigned or passed to functions
- **Composition Over Inheritance**: Achieved through struct embedding
- **Export Control**: Uppercase fields/methods are exported
- **Struct Tags**: Add metadata for JSON/XML encoding and other purposes
- **Memory Efficiency**: Structs allocate memory in a single block
