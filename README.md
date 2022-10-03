# Udemy_Go_Programming_Golang_The_Complete_Developers_Guide_2022-4 by ztm

## why go ?

![1](images/1.png)
![2](images/2.png)

## packages

![3](images/3.png)
![4](images/4.png)
![5](images/5.png)

## Modules
![6](images/6.png)
![7](images/7.png)

## Date types
![8](images/8.png)
![9](images/9.png)
![10](images/10.png)

**type aliases**
![11](images/11.png)

**type conversions**
![12](images/12.png)

### Strings Runes
 
![13](images/13.png)
![14](images/14.png)

in go when we work with strings and runes we're actually working with individual bites, not letters themselves

**Runes**
![15](images/15.png)
![16](images/16.png)

**Strings**
![17](images/17.png)
![18](images/18.png)
![19](images/19.png)

## Go CLI
![20](images/20.png)

----------
# Fundamentals 

## Variables

```go
// compound creation
var a, b, c = 1, 2, "sample"

// Block Creation
var (
	a int = 1
	b int = 2
	// type inference
	c = "sample"
)

// Create & Assign shortcut
example := 3
a, b := 1, "sample"

word1, word2, _ := "one", "two" , "!"
```
![21](images/21.png)
![22](images/22.png)
![23](images/23.png)

## Functions

![24](images/24.png)

```go
func sum(a int, b int) int{
	return a + b
}

func sum(a, b int) int{
	return a + b
}
```
**Multiple Return Values**
![25](images/25.png)

**Go function naming convention is camelCase**

## Operators

![26](images/26.png)
![27](images/27.png)

## if..else
![28](images/28.png)

**statement initialization** allows us to create a variable and do a comparison an the same time.
![29](images/29.png)

**Early Return**
![30](images/30.png)
![31](images/31.png)

## Switch
![32](images/32.png)
![33](images/33.png)

**Conditional Case**
![34](images/34.png)

**Case List**
![35](images/35.png)

**FAllthrough**
![36](images/36.png)

![37](images/37.png)

## Loops
![38](images/38.png)
![39](images/39.png)
**For: While**
![40](images/40.png)
**For: Infinite**
![41](images/41.png)
**Continue**
![42](images/42.png)

![43](images/43.png)

----------

# Types

## Structures
![44](images/44.png)
**Defining a Structure**
![45](images/45.png)
**instantiating a structure**
![46](images/46.png)
**Default Values**
![47](images/47.png)
**Accessing Fields**
![48](images/48.png)
**Anonymous Structures**
![49](images/49.png)

## Arrays
![50](images/50.png)
**Visualization**
![51](images/51.png)
**Creating an Array**
![52](images/52.png)
**Accessing Array Elements**
![53](images/53.png)
**Iteration**
![54](images/54.png)
**Bounds**
![55](images/55.png)
![56](images/56.png)

## Slices
![57](images/57.png)
**Visualization**
slice is not copy the array , just a reference
![58](images/58.png)
**Creating a Slice**
![59](images/59.png)
**Slice Syntax**
![60](images/60.png)
**Example**
![61](images/61.png)
**Dynamic Arrays**
![62](images/62.png)
**Preallocation**
![63](images/63.png)
**Slices to Functions**
![64](images/64.png)
**Multidimensional Slices**
![65](images/65.png)
![66](images/66.png)

## Ranges

```go
func main() {
	slice := []string{"Hello", "world", "!"}
	for i, el := range slice {
		fmt.Println(i, el, ":")
		for _, ch := range el {
			fmt.Printf("%q\n", ch)

		}
	}
}
```

## Maps
![67](images/67.png)
**Making a Map**
![68](images/68.png)
**Map Operations**
![69](images/69.png)
**Iteration**
![70](images/70.png)

## Pointers
![71](images/71.png)
![72](images/72.png)
**creating Pointers**
![73](images/73.png)
**Using Pointers**
![74](images/74.png)
**Pointers Visualized**
![75](images/75.png)
![76](images/76.png)

----------
#  Idiomatic Go

## Receiver Functions
![77](images/77.png)
![78](images/78.png)
![79](images/79.png)
![80](images/80.png)

## iota
**About**
![81](images/81.png)
**iota**
![82](images/82.png)
**Skipping values**
![83](images/83.png)
**iota Enumeration Pattern**
![84](images/84.png)
![85](images/85.png)
**Recap**
![86](images/86.png)

## Variadics

so Variadics are just a way to make a function that accepts any number of parameters.

```go
func sum(nums ...int) int {
	// the nums variable is actually slice of int
	sum :=0
	for _,n := range nums{
		sum += n
	}
	return sum
}

a := []int{1,2,3}
result := sum(a...)
```

## Text Formatting: fmt
![87](images/87.png)
**Printf**
![88](images/88.png)
**Escape Sequences**
![89](images/89.png)
![90](images/90.png)

## Packages 

**Public and Private in go is essentially based on whether or not you capitalize the first letter of a function of structure or type alias**

## init Function
![91](images/91.png)
**Each package can have it's own inti() function**
**All packages will execute init() before main() runs**

## Testing
![92](images/92.png)
![93](images/93.png)
**Available Testing Function**
![94](images/94.png)
**Test Table**
![95](images/95.png)
![96](images/96.png)
![97](images/97.png)

---------------------
# Interfaces
![98](images/98.png)
**Creation & Implementation**
![99](images/99.png)
**Notes**
![100](images/100.png)
**Pass By value vs Pointer**
![101](images/101.png)


