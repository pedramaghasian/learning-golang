## A Quick Introduction to Go

### Introducing Go

- Go is an open-source systems programming language.
- public back in 2009.
- Robert Griesemer, Ken Thomson, and Rob Pike.
-  Go is a general-purpose programming language

**Go does better than other programming language :**

1. The default behavior of the Go compiler can catch a large set of silly errors that might result in bugs.
2. Go uses fewer parentheses than C, C++, or Java, and no semicolons, which makes the look of Go source code more human-readable and less errorprone.
3. Go comes with a rich and reliable standard library.
4. Go has support for concurrency out of the box through goroutines and channels.
5. Goroutines are really lightweight. You can easily run thousands of goroutines on any modern machine without any performance issues.
6. Unlike C, Go supports functional programming.
7. Go code is backward compatible, which means that newer versions of the Go
compiler accept programs that were created using a previous version of the
language without any modifications. This compatibility guarantee is limited
to major versions of Go. For example, there is no guarantee that a Go 1.x
program will compile with Go 2.x.

### The history of Go

- Go started as an internal Google project.
-  Go as a language for professional programmers who want to build .reliable, robust, and efficient software. 
- the Go compiler can find difficult to catch mistakes such as race
conditions.
  
### The advantages of Go

- Go has reserved only 25 keywords.
- concurrency capabilities using a simple concurrency model that
is implemented using goroutines and channel.
- Go manages OS threads for you
and has a powerful runtime that allows you to spawn lightweight units of work (goroutines) that communicate with each other using channels.
- Go's executable binaries are statically
linked, which means that once they are generated, they do not depend on any shared libraries and include all required information.
-  does not have strange side effects, and although Go supports pointers.
-  Go is not an object-oriented programming language, Go interfaces
are very versatile and allow you to mimic some of the capabilities of object-oriented languages such as polymorphism, encapsulation, and composition.
-  the latest Go versions offer support for generics, which simplifies your code when working with multiple data types.

**Although Go is a very practical and competent programming language, it is not perfect:**

-  Go has no direct support for object-oriented programming,
which is a popular programming paradigm.
- Although goroutines are lightweight, they are not as powerful as OS threads.
- Go will not allow you to perform any memory management manually

### The go doc and godoc utilities

- `go doc fmt.Printf`
- `go doc fmt`

- `go install golang.org/x/tools/cmd/godoc@latest`
-  `godoc -http=:8001`

###  Introducing functions

there is a global Go rule that also applies to function and variable names and is valid for all packages except main:
**everything that begins with a lowercase letter is considered private and is accessible in the current package only**

### Introducing packages

- Go programs are organized in packages—even the smallest Go program should be delivered as a package.

- if you are creating an executable application and not just a package that will be shared by other
applications or packages, you should name your package main

### Running Go code

- there are two ways to execute Go code:
as a compiled language using `go build` or as a scripting language using `go run`.

### Compiling Go codes

` go build -o helloWorld hw.go`

### Using Go like a scripting language

The go run command builds the named Go package, which in this case is the main package implemented in a single file, creates a       temporary executable file, executes
that file, and deletes it once it is done—to our eyes, this looks like using a scripting
language.

`go run hw.go`

### Defining and using variables

-  if no initial value is given to a variable,
the Go compiler will automatically initialize that variable to the zero value of its
data type.

-  The official name for := is short assignment statement.
-  the var keyword is mostly used for
declaring global or local variables without an initial value.
- every statement that exists outside of the code of a function must begin with a keyword such as func or var.
- the short assignment statement cannot be used outside of a function environment because it is not available there.
- Global variables can be accessed from anywhere in a
package without the need to explicitly pass them to a function and can be changed unless they were defined as constants using the const keyword.

### Controlling program flow

```go
// With expression after switch
switch argument {
case "0":
fmt.Println("Zero!")
fallthrough
default:
fmt.Println("Value:", argument)
}
```

```go
// No expression after switch
switch {
case value == 0:
fmt.Println("Zero!")
case value > 0:
fmt.Println("Positive integer")
case value < 0:
fmt.Println("Negative integer")
default:
fmt.Println("This should not happen:", value)
}
```

### Iterating with for loops and range

- depending on how you write a for loop, it can function as
a while loop or an infinite loop. Moreover, for loops can implement the functionality
of JavaScript's forEach function when combined with the range keyword.

-  for and range allow you to iterate over the elements
of a map in a similar way.

```go
// Traditional for loop
for i := 0; i < 10; i++ {
fmt.Print(i*i, " ")
}


// For loop used as while loop
i := 0
for {
if i == 10 {
break
}
fmt.Print(i*i, " ")
i++
}

// This is a slice but range also works with arrays
aSlice := []int{-1, 2, 1, -1, 2, -2}
for i, v := range aSlice {
fmt.Println("index:", i, "value: ", v)
}
```

### Reading from standard input

- `fmt.Scanln()`
-  read user input while the program is
already running and store it to a string variable, which is passed as a pointer to
fmt.Scanln().

### Working with command-line arguments

- command-line arguments
in Go are stored in the os.Args slice. Go also offers the flag package for parsing
command-line arguments, but there are better and more powerful alternatives.

```go
	arguments := os.Args
	var number float64
	for i := 1; i < len(arguments); i++ {

		n, err := strconv.ParseFloat(arguments[i], 64)
		if err != nil {
			continue
		}
		number = n
	}
```

### Understanding the Go concurrency model

- The Go concurrency model is implemented using goroutines and channels.
- A goroutine is the smallest executable Go entity.
- In order to create a new goroutine, you have to use
the go keyword followed by a predefined function or an anonymous function.
- A channel in Go is a mechanism that, among other things, allows goroutines to communicate and exchange data.
-  goroutines are do not share any variables, they can share memory.

### sDeveloping the which(1) utility in Go

- Go can work with your operating system through a set of packages.

```go
package main

import (
	"fmt"
	"os"
	"path/filepath"
)

func main() {
	arguments := os.Args
	if len(arguments) == 1 {
		fmt.Println("Please provide an argument!")
		return
	}
	file := arguments[1]
	path := os.Getenv("PATH")
	pathSplit := filepath.SplitList(path)
	for _, directory := range pathSplit {
		fullPath := filepath.Join(directory, file)
		// Does it exist?
		fileInfo, err := os.Stat(fullPath)
		if err == nil {
			mode := fileInfo.Mode()
			// Is it a regular file?
			if mode.IsRegular() {
				// Is it executable?
				if mode&0111 != 0 {
					fmt.Println(fullPath)
					return
				}
			}
		}
	}
}
```

### Logging information

- As we usually run our services via systemd, programs should log to stdout so systemd can put logging data in the journal.
- The UNIX logging service has support for two properties named logging level and logging facility
-  The logging level is a value that specifies the severity of the log entry. There are various logging levels, including debug, info, notice, warning, err, crit, alert, and emerg, in reverse order of severity
-  The log package of the standard Go library does not support working with logging levels.
- The logging facility is like
a category used for logging information. The value of the logging facility part can be
one of auth, authpriv, cron, daemon, kern, lpr, mail, mark, news, syslog, user, UUCP,
local0, local1, local2, local3, local4, local5, local6, or local7 and is defined
inside /etc/syslog.conf, /etc/rsyslog.conf, or another appropriate file.
-  if a logging facility is not defined correctly, it will not be handled.
-  The log package sends log messages to standard error.
-  Part of the log package is the
log/syslog package, which allows you to send log messages to the syslog server of your machine.
- **Logging is for application code, not library code. If you are developing libraries, do not put logging in them.**

- how to use the log and log/syslog packages to log messages to the system syslog on a Unix-like operating system :
```go
package main
import (
"log" //  This is the standard Go package for logging.
"log/syslog" // This package provides functionality to interact with the system's syslog facility.
)
func main() {
// LOG_SYSLOG :This specifies the facility for logging
sysLog, err := syslog.New(syslog.LOG_SYSLOG, "systemLog.go")
if err != nil {
log.Println(err)
return
} else {
log.SetOutput(sysLog)
log.Print("Everything is fine!")
}
}
```
### log.Fatal() and log.Panic()

- The log.Fatal() function is used when something erroneous has happened and you just want to exit your program as soon as possible after reporting that bad situation.
- The call to log.Fatal() terminates a Go program at the point where log.Fatal() was called after printing an error message.
-  it returns back a non-zero exit code, which in UNIX indicates an error.

- There are situations where a program is about to fail for good and you want to
have as much information about the failure as possible—log.Panic() implies that something really unexpected and unknown.
- log.Panic() prints a custom message and immediately
terminates the Go program.
- **log.Panic() is equivalent to a call to log.Print() + panic(). panic() > stops the execution of the current function and begins panicking. After that, it returns to the caller function**

- **log.Fatal() is equivalent to a call to log.Print() + os.Exit(1) immediate way of terminating the current program**

### Overview of Go generics

- Go supports multiple data types in functions such as fmt.Println()
using the empty interface and reflection.
- generics comes into play for providing an alternative to the use
of interfaces and reflection for supporting multiple data types. 
```go
package main
import (
"fmt"
)
func Print[T any](s []T) {
for _, v := range s {
fmt.Print(v, " ")
}
fmt.Println()
}
func main() {
Ints := []int{1, 2, 3}
Strings := []string{"One", "Two", "Three"}
Print(Ints)
Print(Strings)
}
```

-  I believe that generics should be used when they can create simpler code and
designs. It is better to have repetitive straightforward code than optimal abstractions that slow down your applications.

---

## Basic Go Data Types

### The error data type

- Go provides a special data type for representing error conditions and error messages named error.
- this means that Go treats errors as values.
- if the value of an error variable is nil, then there was no error
- if you want to create your own error messages, you can use `errors.New()` from the errors package.
- This usually happens inside a function other than main()
because main() does not return anything to any other function. 
- You will most likely work with errors in your programs without
needing the functionality of the errors package. Additionally,
you do not need to define custom error messages unless you are
creating big applications or packages.
- If you want to format your error messages in the way `fmt.Printf()` works, you can use the `fmt.Errorf()` function, which simplifies the creation of custom error messages—the `fmt.Errorf()` function returns an error value just like `errors.New()`.

```go

package main
import (
"errors"
"fmt"
"os"
"strconv"
)

return errors.New("this is a custom error message")

return fmt.Errorf("a %d and b %d. UserID: %d", a, b, os.Getuid())
```

**you should have a global error handling tactic in each application that should not change. In practice, this means the following:**

- All error messages should be handled at the same level, which means that
all errors should either be returned to the calling function or be handled at the place they occurred.
- It is considered a good practice to send all error messages to the log service of your machine

### Numeric data types

| Data Type  | Description                       |
| ---------- | --------------------------------- |
| int8       | 8-bit signed integer              |
| int16      | 16-bit signed integer             |
| int32      | 32-bit signed integer             |
| int64      | 64-bit signed integer             |
| int        | 32- or 64-bit signed integer      |
| uint8      | 8-bit unsigned integer            |
| uint16     | 16-bit unsigned integer           |
| uint32     | 32-bit unsigned integer           |
| uint64     | 64-bit unsigned integer           |
| uint       | 32- or 64-bit unsigned integer    |
| float32    | 32-bit floating-point number      |
| float64    | 64-bit floating-point number      |
| complex64  | Complex number with float32 parts |
| Complex128 | Complex number with float64 parts |

- The int and uint data types are special as they are the most efficient sizes for signed
and unsigned integers on a given platform and can be either 32 or 64 bits each—their size is defined by Go itself. 

### Non-numeric data types

- Go has support for Strings, Characters, Runes, Dates, and Times. 
-For Go, dates and times are the same thing and are represented by
the same data type.

#### Strings, Characters, and Runes:

- A Go string is just a collection of bytes and can be accessed as a whole or as an array.

- ASCII: ASCII is a 7-bit encoding system, which means it uses 7 bits (128 possible values) to represent characters. It was originally designed for encoding basic English characters, control characters (e.g., carriage return, line feed), and some common symbols.
- A single byte can store any ASCII character.
  
- Unicode: Unicode, on the other hand, is a much more comprehensive character encoding system. It uses 16 bits (65536 possible values) to represent characters, allowing it to encompass a vast range of characters from various scripts and languages around the world.

- multiple bytes are usually needed for storing a single Unicode character.

- Go is designed with Unicode support in mind. which is the main reason for having the rune data type.
  
- A rune is an int32 value that is used for representing a single
Unicode code point.

- Although a rune is an int32 value, you cannot compare a rune
with an int32 value. Go considers these two data types as totally
different.

- You can create a new byte slice from a given string by using a []byte("A String") statement. ( byte slices that contain Unicode characters).
- You can define a rune using single quotes: `r := '€' `and you can print the integer value of the bytes that compose it as `fmt.Println(r)`
- Printing it as a single Unicode character requires the use of the `%c` control string in `fmt.Printf()`.
- The length of the string is the same as the number of characters found in the string,which is usually not true for byte slices because Unicode characters usually require more than one byte.
  

```go

package main

import "fmt"

func main() {

    // a string literal that contains a Unicode character
	aString := "Hello World! €"
	fmt.Println("First character", aString[0])         
	fmt.Println("First character", string(aString[0])) 

	// A rune
	r := '€'
	fmt.Println("As an int32 value:", r)
	// Convert Runes to text
	fmt.Printf("As a string: %s and as a character: %c\n", r, r)

	// Print an existing string as runes
	for _, v := range aString {
		fmt.Printf("%x ", v)
	}
	fmt.Println()

	// Print an existing string as characters
	for _, v := range aString {
		fmt.Printf("%c", v)
	}
	fmt.Println()

}
```

#### Converting from int to string

1. using string()
2. using a function from the strconv package

**However, the two methods are fundamentally different. The `string()` function converts an integer value into a Unicode code point, which is a single character, whereas functions such as `strconv. FormatInt()` and `strconv.Itoa()` convert an integer value into a string value with the same representation and the same number of characters.**

```go
// n = 100
input := strconv.Itoa(n) // "100"
input = strconv.FormatInt(int64(n), 10) // "100"
input = string(n) // "d"
```

#### The unicode package

- The unicode standard Go package contains various handy functions for working with Unicode code points.
- `unicode.IsPrint()`, can help you to identify the parts of a string that are printable using runes.
- This utility is very handy for filtering your input or filtering data before printing it on screen, storing it in log files transferring it on a network, or storing it in a database.


```go
package main
import (
    "fmt"
    "log"
    "unicode"
)
func main() {
    dataToLog := "This is some log data with non-printable characters: \x07"
    if !unicode.IsPrint(dataToLog) {
        log.Fatal("Log data contains non-printable characters.")
    }
    log.Println(dataToLog)
}
```
#### The strings package
- If you are working with text and text processing, you definitely need to learn all the gory details and functions of the strings package
- https://github.com/mactsouk/mastering-Go-3rd/blob/main/ch02/useStrings.go
- https://pkg.go.dev/strings

### Times and dates
- The king of working with times and dates in Go is the `time.Time` data type, which represents an instant in time with *nanosecond precision*. Each `time.Time` value is associated with a **location (time zone)**.
- **when you want to parse a date or time string, you need to use a specific format string to match the structure of the input string. It mentions that if you have a date string like "30 January 2020," you should use the format "02 January 2006" to parse it correctly**
- The next table shows the most widely used strings for parsing dates and times:

| Parse Value | Meaning (examples)                   |
| ----------- | ------------------------------------ |
| 05          | 12-hour value (12pm, 07am)           |
| 15          | 24-hour value (23, 07)               |
| 04          | Minutes (55, 15)                     |
| 05          | Seconds (5, 23)                      |
| Mon         | Abbreviated day of week (Tue, Fri)   |
| Monday      | Day of week (Tuesday, Friday)        |
| 02          | Day of month (15, 31)                |
| 2006        | Year with 4 digits (2020, 2004)      |
| 06          | Year with the last 2 digits (20, 04) |
| Jan         | Abbreviated month name (Feb, Mar)    |
| January     | Full month name (July, August)       |
| MST         | Time zone (EST, UTC)                 |

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    inputDate := "30 January 2020"
    format := "02 January 2006" // write this format from table above.
    parsedDate, err := time.Parse(format, inputDate)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Parsed Date:", parsedDate.Format("02 January 2006"))
}
```

#### A utility for parsing dates and times

```go
package main
import (
"fmt"
"os"
"time"
)

// Is this a date only?
d, err := time.Parse("02 January 2006", dateString)
if err == nil {
fmt.Println("Full:", d)
fmt.Println("Time:", d.Day(), d.Month(), d.Year())

// Is this a date + time?
d, err = time.Parse("02 January 2006 15:04", dateString)
if err == nil {
fmt.Println("Full:", d)
fmt.Println("Date:", d.Day(), d.Month(), d.Year())
fmt.Println("Time:", d.Hour(), d.Minute())
}

// Is this a date + time with month represented as a number?
d, err = time.Parse("02-01-2006 15:04", dateString)
if err == nil {
fmt.Println("Full:", d)
fmt.Println("Date:", d.Day(), d.Month(), d.Year())
fmt.Println("Time:", d.Hour(), d.Minute())
}

// Is it time only?
d, err = time.Parse("15:04", dateString)
if err == nil {
fmt.Println("Full:", d)
fmt.Println("Time:", d.Hour(), d.Minute())
}
```
#### Working with different time zones

1.  you need time.Parse() in order to convert a valid input into a time.Time value before doing the conversions. This time the input string contains the time zone and is parsed by the *"02 January 2006 15:04 MST*" string.
2.  
```go
  package main

import (
	"fmt"
	"os"
	"time"
)

func main() {

	if len(os.Args) == 1 {
		fmt.Println("Need input in __02 January 2006 15:04 MST__ format!")
		return
	}

	input := os.Args[1]
	now, err := time.Parse("02 January 2006 15:04 MST", input)
	if err != nil {
		fmt.Println(err)
		return
	}

	// Local time
	loc, _ := time.LoadLocation("Local")
	fmt.Printf("Current Location: %s\n", now.In(loc))

	// NY
	loc, _ = time.LoadLocation("America/New_York")
	fmt.Printf("New York Time: %s\n", now.In(loc))

	// London
	loc, _ = time.LoadLocation("Europe/London")
	fmt.Printf("London Time: %s\n", now.In(loc))

	// Tokyo
	loc, _ = time.LoadLocation("Asia/Tokyo")
	fmt.Printf("Tokyo Time: %s\n", now.In(loc))
}
```

### Go constants

- Go supports constants, which are variables that cannot change their values.
- the value of a constant variable is defined at compile time, not at runtime—this means that it is included in the binary executable.
- Behind the scenes, Go uses Boolean, string, or number as the type for storing constant values because this gives Go more flexibility when dealing with constants.

#### The constant generator iota

- Although we are defining constants inside main(), constants can be normally found outside of main() or any other function or method.
- The constant generator iota is used for declaring a sequence of related values that use incrementing numbers without the need to explicitly type each one of them.

```go
const PI = 3.1415926
const (
C1 = "C1C1C1"
C2 = "C2C2C2"
C3 = "C3C3C3"
)


const (
Zero int = iota
One
Two
Three
Four
)
const (
Zero = 0
One = 1
Two = 2
Three = 3
Four = 4
)

// constants using bitwise left shift (<<) operations and the iota enumerator
const (
    p2_0 Power2 = 1 << iota // 1 << 0 = 1
    _                      // Blank identifier (_) discards the value
    p2_2 Power2 = 1 << iota // 1 << 2 = 4
    _                      // Blank identifier (_) discards the value
    p2_4 Power2 = 1 << iota // 1 << 4 = 16
    _                      // Blank identifier (_) discards the value
    p2_6 Power2 = 1 << iota // 1 << 6 = 64
)
```

### Grouping similar data
- Go provides an alternative to arrays that is called a **slice**.
- The quick answer is that you can use slices instead of arrays almost anywhere in Go but we are also demonstrating arrays because they can still be useful and because slices are implemented by Go using arrays.

#### Arrays
- When defining an array variable, you must define its size Otherwise, you should put `[...]` in the array declaration and let the Go compiler find out the length for you.
- **If you put nothing in the square brackets, then a slice is going to be created instead**
```go
[4]string{"Zero", "One", "Two", "Three"}
[...]string{"Zero","One", "Two", "Three"}
```
- You cannot change the size of an array after you have created it.
- When you pass an array to a function, what is happening is thatGo creates a copy of that array and passes that copy to that function.
- **As a result, arrays in Go are not very powerful, which is the main reason that Go has introduced an additional data structure named slice**
  
#### Slices
- they are dynamic, which means that they can grow or shrink after creation if needed.
- **any changes you make to a slice inside a function also affect the original slice**.
- **all parameters in Go are passed by value—there is no other way to pass parameters in Go.**
- **In reality, a slice value is a header that contains a pointer to an underlying array where the elements are actually stored, the length of the array, and its capacity.Note that the slice value does not include its elements, just a pointer to the underlying array. So, when you pass a slice to a function, Go makes a copy of that header and passes it to the function. This copy of the slice header includes the pointer to the underlying array.**
- That slice header is defined in the reflect package (https:// golang.org/pkg/reflect/#SliceHeader)
- A side effect of passing the slice header is that it is faster to pass a slice to a function because Go does not need to make a copy of the slice and its elements, just the slice header.

```go
type SliceHeader struct {
Data uintptr
Len int
Cap int
}
```

- If you do not want to initialize a slice, then using `make()` is better and faster. `make([]float64, 3)`
- if you want to initialize it at the time of creation, then `make()` cannot help you. As a result, you can create a slice with three float64 elements as `aSlice :=[]float64{1.2, 3.2, -4.5}`.
  
- Both slices and arrays can have many dimensions:
  1. `make([][]int, 2)`
  2. `twoD := [][]int{{1, 2, 3}, {4, 5, 6}}`

- You can find the length of an array or a slice using `len() ` . 
- You can add new elements to a full slice using the `append()`.
- **`append()` automatically allocates the required memory space.**
- The `append()` commands add two new elements to aSlice. You should save the return value of `append()` to an existing variable or a new one. 

```go
package main

import "fmt"

func main() {
	// Create an empty slice
	aSlice := []float64{}
	// Both length and capacity are 0 because aSlice is empty
	fmt.Println(aSlice, len(aSlice), cap(aSlice))

	// Add elements to a slice
	aSlice = append(aSlice, 1234.56)
	aSlice = append(aSlice, -34.0)
	fmt.Println(aSlice, "with length", len(aSlice))

	// A slice with length 4
	t := make([]int, 4)
	t[0] = -1
	t[1] = -2
	t[2] = -3
	t[3] = -4
	// Once a slice has no place left for more elements, you should add new elements to it using append().
	t = append(t, -5)
	fmt.Println(t)

	// A 2D slice
	// You can have as many dimensions as needed
	twoD := [][]int{{1, 2, 3}, {4, 5, 6}}

	// Visiting all elements of a 2D slice
	// with a double for loop
	for _, i := range twoD {
		for _, k := range i {
			fmt.Print(k, " ")
		}
		fmt.Println()
	}

	make2D := make([][]int, 2)
	fmt.Println(make2D)
	make2D[0] = []int{1, 2, 3, 4}
	make2D[1] = []int{-1, -2, -3, -4}
	fmt.Println(make2D)
}
```

#### About slice length and capacity
- Both arrays and slices support the `len()` function for finding out their length.
- slices also have an additional property called **capacity** that can be found using the `cap()` function.
- **The capacity shows how much a slice can be expanded without the need to allocate more memory and change the underlying array.**
- Although after slice creation the capacity of a slice is handled by **Go**, a developer can define the capacity of a slice at creation time using the `make()` function.
- **the capacity of the slice doubles each time the length of the slice is about to become bigger than its current capacity.**
- ` make([]int, 3, 6)`
- But what happens when you want to append a slice or an array to an existing slice? Should you do that element by element? Go supports the ... operator, which is used for exploding a slice or an array into multiple arguments before appending it to an existing slice.
<div style="text-align:center">
  <img src="./images3/1.png" alt="1" width="500"/>
</div>

```go
package main
import "fmt"
func main() {
	// Only length is defined. Capacity = length
	a := make([]int, 4)
	fmt.Println("L:", len(a), "C:", cap(a))
	// Initialize slice. Capacity = length
	b := []int{0, 1, 2, 3, 4}
	fmt.Println("L:", len(b), "C:", cap(b))
	// Same length and capacity
	aSlice := make([]int, 4, 4)
	fmt.Println(aSlice)
	// Add an element
	aSlice = append(aSlice, 5)
	fmt.Println(aSlice)
	// The capacity is doubled
	fmt.Println("L:", len(aSlice), "C:", cap(aSlice))
	// Now add four elements
	aSlice = append(aSlice, []int{-1, -2, -3, -4}...)
	fmt.Println(aSlice)
	// The capacity is doubled
	fmt.Println("L:", len(aSlice), "C:", cap(aSlice))
}
```
**Setting the correct capacity of a slice, if known in advance, will make your programs faster because Go will not have to allocate a new underlying array and have all the data copied over.**

#### Selecting a part of a slice
- using `aSlice[0:2:4]` selects the first 2 elements of a slice (at indexes 0 and 1) and creates a new slice with a maximum capacity of 4

```go
package main
import "fmt"
func main() {
	aSlice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	fmt.Println(aSlice)
	l := len(aSlice)

	// First 5 elements
	fmt.Println(aSlice[0:5])
	// First 5 elements
	fmt.Println(aSlice[:5])

	// Last 2 elements
	fmt.Println(aSlice[l-2 : l])

	// Last 2 elements
	fmt.Println(aSlice[l-2:])

	// First 5 elements
	t := aSlice[0:5:10]
	fmt.Println(len(t), cap(t))

	// Elements at indexes 2,3,4
	// Capacity will be 10-2
	t = aSlice[2:5:10]
	fmt.Println(len(t), cap(t))

	// Elements at indexes 0,1,2,3,4
	// New capacity will be 6-0
	t = aSlice[:5:6]
	fmt.Println(len(t), cap(t))
}
```

#### Byte slices
- There is nothing special in the way you can access a byte slice compared to the other types of slices.
- What is special is that Go uses byte slices for performing file I/O operations because they allow you to determine with precision the amount of data you want to read or write to a file. This happens because bytes are a universal unit among computer systems.
- **As Go does not have a char data type, it uses byte and rune for storing character values. A single byte can only store a single ASCII character whereas a rune can store Unicode characters**

```go
package main
import "fmt"
func main() {
	// Byte slice
	b := make([]byte, 12)
	fmt.Println("Byte slice:", b)
	b = []byte("Byte slice €")
	fmt.Println("Byte slice:", b)

	// Print byte slice contents as text
	fmt.Printf("Byte slice as text: %s\n", b)
	fmt.Println("Byte slice as text:", string(b))

	// Length of b
	fmt.Println("Length of b:", len(b))
}
```
#### Deleting an element from a slice

- There is no default function for deleting an element from a slice, which means that if you need to delete an element from a slice, you must write your own code.

<div style="text-align:center">
  <img src="./images3/2.png" alt="2" width="500"/>
</div>

```go

// approach one for delete from slice
aSlice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8}
i := 2
aSlice = append(aSlice[:i], aSlice[i+1:]...)

// approach Two for delete from slice
// Replace element at index i with last element
aSlice[i] = aSlice[len(aSlice)-1]
// Remove last element
aSlice = aSlice[:len(aSlice)-1]
```

#### How slices are connected to arrays

- **behind the scenes, each slice is implemented using an underlying array. The length of the underlying array is the same as the capacity of the slice and there exist pointers that connect the slice elements to the appropriate array elements.**
- when the capacity of the slice changes, the connection to the array ceases to exist! This happens because when the capacity of a slice changes, so does the underlying array, and the connection between the slice and the original array does not exist anymore.

#### The copy() function
- `func copy(dst, src []Type) int`.
- The copy() function in Go is used to copy data from one array or slice to another.
- Destination Slice Size: When using copy(), you need to specify the destination slice and the source data to copy. If the source slice is larger than the destination slice, only as much data as the destination can hold will be copied. The remaining data in the source will not be copied.
- Source Slice Remains Unchanged: The source slice remains unchanged after the copy operation. Only the destination slice gets modified.
- No Automatic Resizing: If the destination slice is smaller than the source slice, the copy() function will not automatically expand the destination slice. It will only copy as much data as the destination can hold.

<div style="text-align:center">
  <img src="./images3/3.png" alt="3" width="500"/>
</div>

#### Sorting slices

- The sort package can sort slices of built-in data types without the need to write any extra code.
- Go provides the sort.Reverse() function for sorting in the reverse order than the default.
- However, what is really interesting is that sort allows you to write your own sorting functions for custom data types by implementing the sort.Interface interface.

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	sInts := []int{1, 0, 2, -3, 4, -20}
	sFloats := []float64{1.0, 0.2, 0.22, -3, 4.1, -0.1}
	sStrings := []string{"aa", "a", "A", "Aa", "aab", "AAa"}

	fmt.Println("sInts original:", sInts)
	sort.Ints(sInts)
	fmt.Println("sInts:", sInts)
	sort.Sort(sort.Reverse(sort.IntSlice(sInts)))
	fmt.Println("Reverse:", sInts)

	fmt.Println("sFloats original:", sFloats)
	sort.Float64s(sFloats)
	fmt.Println("sFloats:", sFloats)
	sort.Sort(sort.Reverse(sort.Float64Slice(sFloats)))
	fmt.Println("Reverse:", sFloats)

	fmt.Println("sStrings original:", sStrings)
	sort.Strings(sStrings)
	fmt.Println("sStrings:", sStrings)
	sort.Sort(sort.Reverse(sort.StringSlice(sStrings)))
	fmt.Println("Reverse:", sStrings)
}
```
### Pointers

### Generating random numbers

## Chapter 3: Composite Data Types
86 (107 / 683)