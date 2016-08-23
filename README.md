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

```
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

```
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

```
var c, python, java bool
var i, j int = 1, 2
var c, python, java = true, false, "no!"
```
>If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

#### Short variables
**Inside a function** (and only there), the := short assignment statement can be used in place of a var declaration with implicit type.

```go
func main() {
	k := 3
	fmt.Println(k)
}
```