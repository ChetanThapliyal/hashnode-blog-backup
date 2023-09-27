---
title: "Operators in Go (Blog 7 of the Go Series)"
seoTitle: "Operators in Go (Blog 7 of the Go Series)"
seoDescription: "Operators are fundamental elements in any programming language, including Go."
datePublished: Fri Aug 11 2023 11:30:09 GMT+0000 (Coordinated Universal Time)
cuid: cll6iaszc001m09l84a7aczrh
slug: operators-in-go-blog-7-of-the-go-series
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691724746979/11c19df4-e952-436a-ab51-5c8979104d4b.png
tags: go, devops, hashnode, 2articles1week, devopscommunity

---

Operators are fundamental elements in any programming language, including Go. They allow you to manipulate values, perform calculations, compare values, and more. In this article, we'll delve into the different types of operators available in Go and explore how they are used in combination with control flow statements.

## Arithmetic Operators

Arithmetic operators are used to perform basic mathematical operations. Let's see how they work:

```go
package main

import "fmt"

func main() {
    a := 10
    b := 3

    fmt.Println(a + b) // Addition: 13
    fmt.Println(a - b) // Subtraction: 7
    fmt.Println(a * b) // Multiplication: 30
    fmt.Println(a / b) // Division: 3
    fmt.Println(a % b) // Modulus: 1

    a++
    fmt.Println(a) // Increment: 11
    b--
    fmt.Println(b) // Decrement: 2
}
```

## Comparison Operators

Comparison operators are used to compare two values or expressions. They return a Boolean value indicating whether the comparison is true or false:

```go
package main

import "fmt"

func main() {
    a := 10
    b := 3

    fmt.Println(a == b) // Equal: false
    fmt.Println(a != b) // Not Equal: true
    fmt.Println(a > b)  // Greater Than: true
    fmt.Println(a < b)  // Less Than: false
    fmt.Println(a >= b) // Greater Than or Equal: true
    fmt.Println(a <= b) // Less Than or Equal: false
}
```

## Logical Operators

Logical operators are used to perform operations on Boolean values. They allow you to combine or negate conditions:

```go
package main

import "fmt"

func main() {
    x := true
    y := false

    fmt.Println(x && y) // Logical AND: false
    fmt.Println(x || y) // Logical OR: true
    fmt.Println(!x)     // Logical NOT: false
}
```

## Assignment Operators

Assignment operators are used to assign values to variables or update their values:

```go
package main

import "fmt"

func main() {
    c := 5

    c += 2 // Equivalent to c = c + 2
    fmt.Println(c) // Output: 7

    c *= 3 // Equivalent to c = c * 3
    fmt.Println(c) // Output: 21

    c %= 4 // Equivalent to c = c % 4
    fmt.Println(c) // Output: 1
}
```

## Other Operators

In addition to the operators mentioned above, Go provides several other operators that serve specific purposes:

* The `&` operator is the address operator, used to obtain the memory address of a variable.
    
* The `*` operator is the dereference operator, used to access the value pointed to by a pointer.
    
* The `<-` operator is used for sending and receiving values on channels.
    
* The `?:` operator is the ternary conditional operator, which allows you to choose between two values based on a condition.
    

```go
package main

import "fmt"

func main() {
    c := 5
    p := &c
    fmt.Println(p)  // Memory address of c
    fmt.Println(*p) // Value of c

    ch := make(chan int)
    go func() {
        ch <- 42
    }()
    fmt.Println(<-ch) // Receive value from channel

    res := ""
    if a > b {
        res = "a > b"
    } else {
        res = "a <= b"
    }
    fmt.Println(res)
}
```

## Conclusion

Understanding operators and control flow is essential for writing efficient and meaningful Go programs. With the knowledge of arithmetic, comparison, logic, assignment, and other operators, you can manipulate values and control the flow of your program's execution. By mastering these concepts, you'll be better equipped to create powerful and expressive Go applications.