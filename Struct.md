```
# Go Structs Examples

## Table of Contents
1. [Basic Struct Declaration](#1-basic-struct-declaration)
2. [Struct Initialization with Literals](#2-struct-initialization-with-literals)
3. [Struct with Methods](#3-struct-with-methods)
4. [Nested Structs](#4-nested-structs)
5. [Struct Tags for JSON Marshaling](#5-struct-tags-for-json-marshaling)
6. [Anonymous Struct](#6-anonymous-struct)
7. [Struct Embedding (Composition)](#7-struct-embedding-composition)
8. [Key Points](#key-points)

---

## 1. Basic Struct Declaration
```go
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
    fmt.Println(p) // {Alice 30 123 Main St}
}
```
Basic struct definition with field declarations and instance creation.

---

## 2. Struct Initialization with Literals
```go
p := Person{
    Name:    "Bob",
    Age:     25,
    Address: "456 Oak Ave",
}
fmt.Printf("%+v\n", p) // {Name:Bob Age:25 Address:456 Oak Ave}
```
Initialize structs using literals with explicit field names for clarity.

---

## 3. Struct with Methods
```go
func (p Person) IsAdult() bool {
    return p.Age >= 18
}

// Usage:
fmt.Println(p.IsAdult()) // true
```
Attach methods to structs using receiver parameters for encapsulated functionality.

---

## 4. Nested Structs
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
    Age: 40,
    Address: Address{
        Street: "789 Pine Rd",
        City: "Tech City",
        Zip: "12345",
    },
}
```
Organize complex data using nested structs for better data modeling.

---

## 5. Struct Tags for JSON Marshaling
```go
type User struct {
    Username string `json:"username"`
    Email    string `json:"email"`
    isAdmin  bool   `json:"-"` // Ignored in JSON
}

u := User{"admin", "admin@example.com", true}
data, _ := json.Marshal(u)
fmt.Println(string(data)) // {"username":"admin","email":"admin@example.com"}
```
Use struct tags to control JSON serialization behavior.

---

## 6. Anonymous Struct
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
fmt.Println(book.Title) // The Go Programming Language
```
Create temporary structs without explicit type definitions.

---

## 7. Struct Embedding (Composition)
```go
type Animal struct {
    Name string
}

func (a Animal) Speak() string {
    return "Some generic sound"
}

type Dog struct {
    Animal // Embedded struct
    Breed  string
}

d := Dog{
    Animal: Animal{Name: "Rex"},
    Breed:  "Golden Retriever",
}
fmt.Println(d.Speak()) // Some generic sound
fmt.Println(d.Name)    // Rex
```
Implement composition through struct embedding for code reuse.

---

## Key Points
- 📦 Structs are value types (copied when passed around)
- 📌 Exported fields start with uppercase letters (required for JSON serialization)
- 🎯 Use struct literals for concise initialization
- 🛠️ Attach methods using receiver parameters
- 🔄 Composition preferred over inheritance via embedded structs
- 🏷️ Struct tags provide metadata for encoding/decoding

---
