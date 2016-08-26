# The Go Programming Language

## Learning notes

### Hello world program
```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World!")
}
```

### Packages
Every Go program is made up of packages. The package of a program file is specified at the top of the file. 

Packages can be imported using the ```import``` directive. Ex:

```go
import (
	"fmt"
	"math/rand"
)
```

### Printing to the console
```go
fmt.Printf("Now you have %g problems.", math.Sqrt(7))
```
Note that the package _fmt_ needs to be imported.


### Public/Private names
In Go we talk about names, an concretely about **exported names** if they **begin with a capital letter**.
> When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package.

Ex: ```Pi``` is an exported name from the ```math``` package. So you can use ```math.Pi``` in your code as well as you have imported the ```math``` package.

### Functions
A function can take zero or more arguments. They have the following structure:

```
func {name}({param1_name} {param1_type}, ...) {return_type, ...} {
	...
}
```
For example:

```go
func add(x int, y int) int {
	return x + y
}
```
Notice that the type comes after the variable name.
If two or more variables share type, you can abbreviate them as ```x, y int ``` instead of ```x int, y int```. 

### Multiple return types
A function can return any number of results. For example:

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```
If return values are names, they can be used as variables inside the funciton body. Then a single ```return```is necessary to return all the value. However, using _naked_ returns is discouraged for readability. Example:

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return x, y
}
```

### Variables
The ```var``` statement declares a list of variables; as in function argument lists, the type is last.
A ```var``` statement can be at package or function level.
Examples:

```go
var c, python, java bool
var i, j int = 1, 2
var c, python, java = true, false, "no!"
```
>If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

You can also declare variables in blocks, as with import statements. Ex:

```go
var (
	finsihed 	bool 			= false
	max			int				= 4
	z			complex128	= cmplx.Sqrt(-5 + 12i)
)
```

#### Short variables
**Inside a function** (and only there), the := short assignment statement can be used in place of a var declaration with implicit type.

```go
func main() {
	k := 3
	fmt.Println(k)
}
```
In case of type inference, the new variable may be an ```int```, ```float64```, or ```complex128``` depending on the precision of the constant.

#### Constants
Declared like variables, but with the ```const```keyword instead of ```var```.
Example:

```go
const PI = 3.14
```
Constants **cannot** be declared using the ```:=``` syntax. However they can be declared inside methods.

#### Basic types

| Type | Description | Default value |
|:---- |:----------- |:--- |
| bool |				 | false |
| int |				 | 0 |
| int8  int16  int32  int64 |				 | 0 |
| uint |				 | 0 |
| uint8 uint16 uint32 uint64 uintptr |	 | 0 |
| byte | alias for uint8 | 0 |
| rune | alias for int32 | 0 |
| float32 float64 | | 0 |
| complex64 complex128 | | 0 |
| string | | ""|

#### Casting
Needs to be explicit as ```T(v)``` where ```T```specifies the new type for the value ```v```. For example:

```go
var i int = 42
var f float64 = float64(i)
```

### Flow control
#### Loops
Go has only one looping construct, the ```for``` loop.

```go
sum := 0
for i := 0; i < 10; i++ {
	sum += i
}
```
> Note that no parentheses are needed but brackets ```{}``.

The init and post statement are **optional** so ```for ; sum < 100; {}``` would also be valid. More over the ```;```are also optional, so you can simulate a traditional ```while``` doing:

```go
sum := 1
for sum < 1000 {
	sum += sum
}
```

#### Conditionals
```go
if x > 10 {
	return "bigger"
}
```
Like ```for```, the ```if``` statement can start with a short statement to execute before the condition. This variable will only be visible inside the ```if```scope. Example:

```go
if v := math.Pow(x, n); v < lim {
	return v
} else {
	return v*2
}
```
>Variables declared inside an if short statement are also available inside any of the else blocks.

#### Switch statement
```go
switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}
```
> Note: the assignation of the variable next to the ```switch``` statement could be changed to ```switch runtime.GOOS {```.

A ```switch```statement without condition is the same as ```switch true```.

#### Defer
A defer statement pospone the execution of a function until the last not defered **funciton** returns.

```go
func main() {
	var a = 2;
	defer fmt.Printf("a value is:  %v\n", a)
	defer fmt.Println("almost the last function")
	
	fmt.Println("hello")
	a = changeA(a)
	fmt.Printf("here, a value is: %v\n", a)
}
```

> The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a **stack**. When a function returns, its deferred calls are executed in last-in-first-out order.

### Data structures

#### Pointers
A pointer holds the memory address of a value. Pointers are declared as ```*T```where ```T``` is the type of the value pointed. Example:

```go
i, j := 42, 2701
p := &i         // point to i
fmt.Println(*p) // read i through the pointer
*p = 21         // set i through the pointer
fmt.Println(i)  // see the new value of i

var secondPointer *int
secondPointer = &j         // point to j
*secondPointer = *secondPointer / 37   // divide j through the pointer
fmt.Println(j) // see the new value of j
```
As seen above, the ```&``` operator generates a pointer to its operand.

> In Go (unlike C) ther's no pointer arithmetic, so you can not do operations with the pointer itself but with its pointed value.

#### Structs
A struct is a collection of fields. It's declared like:

```
type {struct_name} struct {
	x int
	...
}
```
Struct fields are accessed as: ```{struct_name}.x```


When a **pointer** ```p``` points to a struct (```p := &v```), we can access to the struct fields directly by using ```p.x```. In this case ```p.x``` is equivalent to ```(*p).x```.

You can initialize a struct using one of the following methods:

```go
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)
```
> When using the subset field syntax, the order of the fields is **irrelevant**


### Tricks
##### Print the type and value of a variable
```go
v := 42 
fmt.Printf("v is of type %T with value %v\n", v, v)
```
#### Time package
```
import "time"
today := time.Now().Weekday()
```