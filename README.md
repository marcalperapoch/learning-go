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

#### Arrays
Arrays in Go are declared with type ```[n]T``` where ```T``` is the type and ```n``` the array size, which is fixed so they **cannot be resized**. Examples:

```go
var a [10]int
primes := [6]int{2, 3, 5, 7, 11, 13}
```
Arrays indexs are **0-th** based.

#### Slices
A slice, is a dynamically-sized, flexible view into the elements of an array. Declared as ```[]T```, it represents a slice with elements of type T.

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
var s []int = primes[1:4]
```
Slices are like projections of an array, they just describe a section of an underlying array.

>Changing the elements of a slice modifies the corresponding elements of its underlying array. Other slices that share the same underlying array will see those changes.

However, Slices can be used as **literals**. Declaring slices this way, automatically creates the underlaying array and builds the slice that references it with the same operation.

```go
[]bool{true, true, false}
q := []int{2, 3, 5, 7, 11, 13}
```

When slicing, you may omit the high or low bounds to use their defaults instead. The default is zero for the low bound (```a[:10]``` is equivalent to ```a[0:10]```]) and the length of the slice for the high bound (```a[1:]``` is equivalent to ```a[1:size]```).

A slice has both a **length** and a **capacity**.

* **length**: is the number of elements it contains.
* **capacity**: is the number of elements in the underlying array, counting from the first element in the slice.

The length and capacity of a slice ```s``` can be obtained using the expressions ```len(s)``` and ```cap(s)```.

You can extend a slice's length by re-slicing it, provided it has sufficient capacity. For example:

```go
s := []int{2, 3, 5, 7, 11, 13}
// Drop its first two values.
s = s[2:]
// runtime error: slice bounds out of range
s = s[:6]
```

The zero value (_length_ and _capacity_ 0) of a slice is ```nil```.

You can also create **dynamically-sized** arrays/**slices** using the ```make``` function.

```go
a := make([]int, 5)  // len(a)=5
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
```

Moreover you can **append new elements** to a slice using the _built-in_ ```append```function.

```go
func append(s []T, vs ...T) []T
```
```go
s = append(s, 2, 3, 4)
```
> Atention: the ```append``` function returns a value! It does not modify the original slice.

#### Matrixs and Slices of slices
Slices can contain any type, including other slices.

```go
board := [][]string{
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
}

// The players take turns.
board[0][0] = "X"
```

#### Arrays iteration
Can be done using the **range** operand of the ```for``` loop.

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
for index, value := range pow {
}
```
When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a **copy** of the element at that index.

You can skip the _index_ or _value_ by assigning to ```_```. Moreover if you only want the index you can remove all the ```,value```part.

#### Maps
The zero value of a map is ```nil```. A ```nil``` map has no keys, nor can keys be added.

To initialize a map, you need to use the ```make```function with the following syntax:
```map[keys_type]values_type```. For example:

```go
type Vertex struct {
	Lat, Long float64
}

var m = make(map[string]Vertex)
m["Bell Labs"] = Vertex{
	40.68433, -74.39967,
}
```
Maps can be initialized at construction time:

```go
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```
The declaration of ```Vertex```inside the map can be infered, so there's no need to declare it again.

```go
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    Vertex{37.42202, -122.08408},
}
```

**Operations**:

* Insert/Update: ```m[key] = elem```
* Get: ```elem = m[key]```
* Delete: ```delete(m, key)```
* Contains: ```elem, ok = m[key]``` or ```elem, ok := m[key]``` if haven't been declared yet

In the case of "contains" operation, if ```key``` is in ```m```, ```ok``` is ```true```. If not, ```ok``` is ```false```.

If ```key``` is not in the map, then ```elem``` is the zero value for the map's element type.

#### Functions

> Functions are values too.

They can be passed around just like other values, for example: as function arguments and return values.

```go
hypot := func(x, y float64) float64 {
	return math.Sqrt(x*x + y*y)
}
fmt.Println(hypot(5, 12))
```

#### Closures
 A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.
 
 ```go
// adder is a function that returns a function (that takes an int param)
// that returns an int
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
 ```

In the example above, the adder function returns a closure. Each closure is bound to its own sum variable.

#### Methods


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