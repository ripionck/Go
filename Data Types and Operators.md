## **1. Basic Data Types**

### **Numeric Types**
**Integers** (Signed/Unsigned):
```go
var a int = 42        // Platform-dependent size (32/64 bits)
var b int32 = -15     // 32-bit signed
var c uint8 = 255     // 8-bit unsigned (0-255)
```

**Floating-Point**:
```go
var f1 float32 = 3.14 // 32-bit
var f2 float64 = 1e9  // 64-bit (default for literals)
```

**Complex Numbers**:
```go
var c1 complex64 = 1 + 2i
var c2 complex128 = complex(3, 4)
```

### **Boolean**
```go
var isReady bool = true
var isActive = false  // Type inference
```

### **Strings**
```go
s1 := "Hello"         // Immutable UTF-8
s2 := `Multiline
string`
s3 := s1 + " World!"  // Concatenation
```

### **Special Types**
```go
var r rune = 'A'      // Alias for int32 (Unicode code point)
var b byte = 0x41     // Alias for uint8 (ASCII)
```

---

## **2. Composite Data Types** (Quick Reference)
| Type      | Example                          |
|-----------|----------------------------------|
| Array     | `var arr [3]int = [3]int{1,2,3}`|
| Slice     | `slice := []string{"a", "b"}`    |
| Map       | `m := map[string]int{"age": 25}` |
| Struct    | `type Point struct { X, Y int }` |

---

## **3. Operators**

### **Arithmetic Operators**
```go
a := 10
b := 3

fmt.Println(a + b)   // 13
fmt.Println(a - b)   // 7
fmt.Println(a * b)   // 30
fmt.Println(a / b)   // 3 (integer division)
fmt.Println(a % b)   // 1
```

### **Comparison Operators**
```go
x := 5
y := 7

fmt.Println(x == y)  // false
fmt.Println(x != y)  // true
fmt.Println(x < y)   // true
fmt.Println(x >= 5)  // true
```

### **Logical Operators**
```go
t := true
f := false

fmt.Println(t && f)  // false
fmt.Println(t || f)  // true
fmt.Println(!t)      // false
```

### **Bitwise Operators**
```go
n := 0b1010          // Binary 10
m := 0b1100          // Binary 12

fmt.Printf("%b\n", n & m)   // 1000 (AND)
fmt.Printf("%b\n", n | m)   // 1110 (OR)
fmt.Printf("%b\n", n ^ m)   // 0110 (XOR)
fmt.Printf("%b\n", n << 2)  // 101000 (Left shift)
```

### **Assignment Operators**
```go
a := 5
a += 3        // a = 8
a *= 2        // a = 16
a <<= 1       // a = 32
```

### **Other Operators**
```go
ptr := &a      // Address-of
*ptr = 42      // Dereference
fmt.Println(a) // 42
```

---

## **4. Type Conversion**
Go requires explicit type conversion:
```go
// Numeric conversion
i := 42
f := float64(i)
u := uint(f)

// String conversion
s := string(65)        // "A" (ASCII)
bytes := []byte("Go")  // [71 111]

// Interface conversion (type assertion)
var val interface{} = "hello"
str := val.(string)    // "hello"
```

---

## **5. Operator Precedence**

| Precedence | Operators                     |
|------------|-------------------------------|
| Highest    | `* / % << >> & &^`            |
|            | `+ - | ^`                     |
|            | `== != < <= > >=`             |
|            | `&&`                          |
| Lowest     | `||`                          |

Example:
```go
result := 5 + 3*2  // 11 (not 16)
```

---

## **Key Points**
- **Zero Values**:
  - Numeric: `0`
  - Boolean: `false`
  - String: `""`
  - Pointers: `nil`

- **No Implicit Conversion**:
  ```go
  var i int = 10
  var f float64 = i  // Error
  var f float64 = float64(i)  // Correct
  ```

- **String Features**:
  - Immutable: `s[0] = 'H'` is invalid
  - UTF-8 encoded: `len("こんにちは")` returns 15 (bytes)

- **Operator Notes**:
  - No operator overloading
  - No ternary operator (`?:`)
  - `++`/`--` are statements, not expressions
