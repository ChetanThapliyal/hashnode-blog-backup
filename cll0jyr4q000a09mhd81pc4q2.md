---
title: "Building Blocks of Go: Variables and Comments (Blog 3 of the Go Series)"
seoTitle: "Variables and Comments in Go"
seoDescription: "Variables, Printing and Comments in Golang"
datePublished: Mon Aug 07 2023 07:30:09 GMT+0000 (Coordinated Universal Time)
cuid: cll0jyr4q000a09mhd81pc4q2
slug: building-blocks-of-go-variables-and-comments-blog-3-of-the-go-series
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691353927124/99b5798e-c3ed-44cd-a87d-f1b2dda2bb39.png
tags: go, coding, devops, basics, scripting-languages

---

### Variables: Building Blocks of Dynamic Data Handling

In the world of computer programming, variables emerge as essential tools for managing and manipulating data. A variable serves as a named storage location that holds a specific value of a particular data type. What's intriguing is that this value can be modified during the program's execution, enabling the storage of data for future use.

Each variable boasts a name, a designated data type, and an associated value. This name serves as a reference point within the program, while the data type specifies the kind of value the variable can store. Notably, a variable's value can be set either during its declaration or later in the program's lifecycle.

Variables play a pivotal role in programming, offering a dynamic means of handling data. They empower programs to adapt and reuse information, eliminating the need for hardcoded values. This flexibility not only enhances a program's versatility but also simplifies future modifications.

### Declaring Variables: Multiple Paths to the Same Destination

In the Go programming language, there exists an array of methods to declare variables, each tailored to specific needs:

1. Using the `var` Keyword: The traditional approach involves the `var` keyword, demanding explicit specification of the data type:
    
    ```go
    var age int = 26
    ```
    

1. Shorthand with `:=`: A more concise method allows you to both declare and initialize a variable, with the data type inferred from the assigned value:
    
    ```go
    name := "John Doe"
    ```
    
2. The `var` Block: For a streamlined declaration of multiple variables within a single block, you can employ the `var` block:
    
    ```go
    var (
      name string = "John Doe"
      age int = 26
    )
    ```
    
3. Constants with `const`: When dealing with values that remain unchanged, the `const` keyword creates constants:
    
    ```go
    const Pi float64 = 3.14
    ```
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Variable Declaration : <a target="_blank" rel="noopener noreferrer nofollow" href="https://github.com/ChetanThapliyal/get-started-with-Go/blob/main/02-Variable-Declaration/variable-declaration.go" style="pointer-events: none">https://github.com/ChetanThapliyal/get-started-with-Go/blob/main/02-Variable-Declaration/variable-declaration.go</a></div>
</div>

Remember, the choice of declaration method hinges on your specific intentions and requirements within the program.

### Print Methods: Expressing Values to the World

In Go, diverse methods facilitate the printing of variable values:

1. `fmt.Println`: This commonly used function prints a variable's value to the console, appending a newline character:
    
    ```go
    var name string = "Harry Potter"
    var house string = "Gryffindor"
    fmt.Println(name)
    fmt.Println(house)
    // OUTPUT: 
    // Harry Potter
    // Gryffindor
    ```
    
2. `fmt.Printf`: With this method, formatted strings are printed, allowing variable values to be integrated using format specifiers:
    
    ```go
    name := "John Doe"
    fmt.Printf("My name is %s\n", name)
    // >>> output
    // My name is John Doe
    ```
    
3. `fmt.Sprintf`: This method formats strings and returns them as values, useful for storing or utilizing formatted strings elsewhere:
    
    ```go
    age := 26
    message := fmt.Sprintf("My age is %d", age)
    fmt.Println(message)
    // >>> output
    // My age is 26
    ```
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Printing Variables: <a target="_blank" rel="noopener noreferrer nofollow" href="https://github.com/ChetanThapliyal/get-started-with-Go/blob/main/03-Printing-Variables/printing-variables.go" style="pointer-events: none">https://github.com/ChetanThapliyal/get-started-with-Go/blob/main/03-Printing-Variables/printing-variables.go</a></div>
</div>

It's worth noting that alternative packages, such as the `log` and `strconv` packages can also aid in printing variables based on distinct programming necessities.

### Comments: Adding Clarity to Your Code

Comments are vital tools for enhancing code readability in Go. They come in two forms:

1. Single Line Comments: Brief explanations are marked with `//`, aiding in providing clarity for individual lines of code:
    
    ```go
    // This is a single line comment
    ```
    
2. Multi-line Comments: Longer explanations, spanning multiple lines, are enclosed within `/*` and `*/`:
    
    ```go
    /*
    This is a
    multi-line comment
    */
    ```
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Comments in Go: <a target="_blank" rel="noopener noreferrer nofollow" href="https://github.com/ChetanThapliyal/get-started-with-Go/blob/main/04-Comments/comments.go" style="pointer-events: none">https://github.com/ChetanThapliyal/get-started-with-Go/blob/main/04-Comments/comments.go</a></div>
</div>

These comments serve as a guide, offering human-readable insights into the code and contributing to a better understanding of its functionality.

In the dynamic world of Go programming, variables and comments unite to shape efficient, comprehensible, and flexible code. These fundamental concepts lay the groundwork for your coding journey, enabling you to craft software that's both expressive and robust.