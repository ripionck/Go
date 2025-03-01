### **Variable Declaration & Initialization**

### **1. Declaration Methods**
**a. Explicit Declaration**
```go
var identifier type       // Declaration without initialization
var identifier type = value // Declaration with initialization
```

**b. Implicit Declaration**
```go
identifier := value       // Type inferred from value (most common)
```

### **2. Examples**
```go
// Basic types
var a int           // Declaration (zero value = 0)
var b int = 10      // Declaration with initialization
c := 20             // Type inferred as int

// Strings
var s string        // Zero value = ""
var t string = "Hello"
u := "World"        // Type inferred as string

// Multiple variables
var x, y int = 5, 10
i, j := 3.14, true  // i=float64, j=bool

// Zero values
var (
    num int     // 0
    str string  // ""
    ok bool     // false
)
```

---

## **Type Conversion**

### **1. Explicit Conversion**
Go requires explicit type conversion between different types.

**Syntax:**
```go
newType(value)
```

### **2. Conversion Examples**
```go
// Numeric conversions
i := 42
f := float64(i)     // int → float64
u := uint(f)        // float64 → uint

// String conversions
str := "Go"
bytes := []byte(str) // string → []byte
str2 := string([]byte{71, 111}) // []byte → string ("Go")

// Precision loss example
pi := 3.14159
intPi := int(pi)    // 3 (truncates decimal)

// Interface conversion (type assertion)
var empty interface{} = "hello"
s := empty.(string) // "hello"
```

### **3. Invalid Conversions**
```go
var flag bool = true
// num := int(flag)   // Compile error: cannot convert bool to int
```

---

## **Type Inference**

### **1. Inference Rules**
- Determined by value on right-hand side
- Default numeric types:
  - Integer: `int`
  - Float: `float64`
  - Complex: `complex128`

### **2. Inference Examples**
```go
a := 42           // int
b := 3.14         // float64
c := 0.867 + 0.5i // complex128
d := true         // bool
s := "hello"      // string

// Expression inference
x := 10 + 3.14    // float64 (int promoted to float64)
y := 5 * 2.5      // float64

// Function return inference
func getData() (int, string) {
    return 42, "answer"
}

num, str := getData()  // num=int, str=string
```

### **3. Default Types Table**
| Value Literal     | Inferred Type |
|-------------------|---------------|
| 42                | int           |
| 3.14              | float64       |
| true/false        | bool          |
| "hello"           | string        |
| 'a'               | rune (int32)  |
| 0i                | complex128    |

---

## **Key Concepts**

### **1. Zero Values**
Automatically assigned when variables are declared without initialization:
- Numeric: `0`
- Boolean: `false`
- Strings: `""`
- Pointers: `nil`
- Slices/Maps/Channels: `nil`

### **2. Type Safety**
- **Strict Typing**: Variables cannot change type after declaration
- **No Implicit Conversion**: All type changes must be explicit
  ```go
  var count int = 10
  // var total float64 = count  // Error
  var total float64 = float64(count) // Correct
  ```

### **3. Best Practices**
1. Use `:=` for local variables when possible
2. Use explicit types for package-level variables
3. Convert early in expressions to prevent type mismatch
4. Handle precision loss in numeric conversions carefully

---

## **Practical Use Cases**

### **1. Math Operations**
```go
a := 10         // int
b := 3.14       // float64

// result := a + b         // Error
result := float64(a) + b  // Valid (float64)
```

### **2. String Formatting**
```go
age := 30
message := "I'm " + strconv.Itoa(age) // int → string conversion
// Alternative: fmt.Sprintf("I'm %d", age)
```

### **3. JSON Handling**
```go
type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
}

data := []byte(`{"id":1,"name":"Alice"}`)
var user User
json.Unmarshal(data, &user) // Automatic type conversion
```

---
