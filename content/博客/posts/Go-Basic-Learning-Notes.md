---
{"publish":true,"title":"Go Basic Learning Notes","created":"2020-03-25 14:09:48","modified":"2025-12-22T09:59:24.104+08:00","tags":["learning"],"cssclasses":""}
---

****![|700x309](https://images2.dzmtoast.top/ob-240907-143147.png)


# Preface

==Let's GO / 即刻出发==

Learning go basics follow steps offered by: https://golangbot.com/learn-golang-series/

Wish me good luck

# CheckList

Introduction
✅1 - Introduction and Installation
✅2 - Hello World

Variables, Types and Constants
✅3 - Variables
✅4 - Types
✅5 - Constants

Functions and Packages
✅6 - Functions
✅7 - Packages

Conditional Statements and Loops
✅8 - if else statement
✅9 - Loops
✅10 - Switch Statement

Arrays, Slices and Variadic Functions
✅11 - Arrays and Slices
✅12 - Variadic Functions

More types
✅13 - Maps
⛷[skip]14 - Strings

Pointers, Structures and Methods
✅15 - Pointers
✅16 - Structures
✅17 - Methods

Interfaces
✅18 - Interfaces - I
✅19 - Interfaces - II

Concurrency
✅20 - Introduction to Concurrency
✅21 - Goroutines
✅22 - Channels
✅23 - Buffered Channels and Worker Pools
✅24 - Select
✅25 - Mutex

Object Oriented Programming
✅26 - Structs Instead of Classes
✅27 - Composition Instead of Inheritance
✅28 - Polymorphism

Defer and Error Handling
✅29 - Defer
✅30 - Error Handling
✅31 - Custom Errors
32 - Panic and Recover

First Class Functions
33 - First Class Functions

Reflection
34 - Reflection

Filehandling
35 - Reading Files
36 - Writing Files

# Why Go ?
--

# Env & Run

``` shell
# run entry
go main.go

# build binary
go build

```

# 变量
## 变量声明


``` go
// 常规式
var (
    myVar1 = "value1",
    myVar2 = "value2"
)

// Ninja 式
myVar1, myVar2 := "value1", "value2"
```

\* `=` 是赋值 / `:=` 是声明变量并赋值, 所以使用 `:=` 要求左边变量至少有一个是新声明的


## 类型

### 类型概览
bool

Numeric Types
- int8, int16, int32, int64, int
- uint8, uint16, uint32, uint64, uint
- float32, float64
- complex64, complex128
- byte // alias to uint8
- rune // alias to int32

string

### 类型要点

\* int 表示 32bit / 64bit integer, 取决于操作系统是 32bit / 64bit, 大多数时候应该使用它, 而不直接指定 int32/int64

\* 复数声明方式 
1. var cplx = complex(10, 20)
2. var cplx = 10 + 20i

实部和虚部是 32bit, 结果是 64bit
实部和虚部是 64bit, 结果是 128bit

\* different types are not allowed to compute together (this part is not like shitty 'typed' js)

\* const 声明的变量不能被重新赋值，而且类型需要在编译器可确定，如 const a = math.Sqrt(2) 是不允许的, 因为 math.Sqrt 在运行时才会确定



# 函数

函数声明起手式

``` go
func fnName (p1, p2 int) int {
    ret := p1 * p2
    return ret
}
```

go 的函数和很多语言不一样，可以返回多个值

``` go
func fnName (p1, p2 int) (int, int) {
    sq1, sq2 := p1 * p1, p2 * p2
    return sq1, sq2
}

sq1, sq2 := fnName(10, 20)
```

也可以不显式 return, 而是在返回值类型定义的地方指明返回的变量名 (相当于已经在返回值类型定义的地方声明了变量)

``` go
func fnName (p1, p2 int) (sq1, sq2 int) {
    // 这里不再能使用 := 了，因为 sq1，sq2相当于已经声明过了
    // 直接赋值↓ 对号入座
    sq1, sq2 = p1 * p1, p2 * p2
    // return sq1, sq2
    return // 但是 return 语句不能少
}

sq1, sq2 := fnName(10, 20)
```


# Package

\* package 里需要暴露出去的变量都以大写开头, 不以大写开头的从外部无法访问

## 初始化
Package 允许带一个 init 函数
``` go
func init () {

}
```

这个函数在 Package 被初始化的时候调用，即便多次 import, 也只会初始化一次

初始化函数不带任何参数，也不会有返回类型（当然了! 模块得保持中立，自然不存在参数化）

# if-statement

other language:

``` js

if (condition) {
    // then
} else {
    // else
}

```

golang has another `extra statement` to go, which u can do some init work, or preparation (u can do similar thing outside of if-statement, but its kinda inelegant)

``` go
if statement; condition {
    // then
}
```

else-statement has to stay on same line with last close-braces and next start-braces

``` go
// allowed syntax
} else {

// not allowed ❎
}
  else {
```

# Loop

\* similar to most other languages ...

\* label: like go-to statement of loop, get out of inner-most loop

\* infinite loop:

``` go
for {
    // ...
}
```

# Switch

\* similar to most other languages ...

switch expression is __OPTIONAL__

``` go
switch statement {
    case xxx:
}
// sort of if-else
switch {
    case xxx:
}

```

\* fallthrough, like a pass ... (传球), it MUST be last statement of a CASE

``` go
switch age {
    case age>10:
        fmt.Println("older than 10")
        fallthrough   // <== here
    case age>15
        fmt.Println("also older than 15")
        break
}
```

# Array

## Declaration

``` go
//        ↓ length of arr
var arr [10]int

arr := [10]int{1,2,3,4} // all zeros after '4'

arr := [...]int{1,2,3} // let compiler tell the length

```

\* [3]int and [5]int are distinct types

## value-type

\* Array in GO is 🚨 **VALUE-TYPE**, assignment of array will result in value copy

so \*swap\* shit wont happen ..

``` go

func swap(arr [2]int) [2]int {
    tmp := arr[0]
    arr[0] = arr[1]
    arr[1] = tmp
    return arr
}
myArr := [...]int{10, 20}
swap(myArr)
// just a copy of myArr passed into swap func, myArr stay unchanged
// u might want to do this ↓
myNewArr := swap(myArr)
```

## iteration

go provide `range` to make iterating more concise

``` go
for idx, value := range arr {
	fmt.Println(idx, value)
}
```

# Slices

> Slice is a data interface of Array, any change to slice will reflected on origin array

slice / array desctructing ->  `array...` ( a bit weird for javascript developer ...)

\* 🤩 change to slice will reflect on original array, so if u pass a slice as parameter to a func, any change to this slice inside func will also reflect on original array

\* use `copy` to make a copy of slice, so underlying array can be GC


# Map

## Basic syntax / usage

``` go
salary := map[string]float64 {
    "Steve": 123.0,
    "Jobs": 66.6, // <-- this tailing comma is *REQUIRED*
}

```

## access / set data:

just like javascript ...

``` go
v := salary["Jobs"]
salary["a-new-employee"] = 10086.0
fmt.Println(salary["Mike"]) // Mike not exist in salary map, zero-value of float type will returned

// to check if a specific key exists on map, or it's exactly zero-value
salary := map[string]float64 {
    "Steve": 123.0,
    "Jobs": 66.6,
    "Boss": 0.0, // <- key *EXISTS*, just exactly zero-value
}
// ↓ *SECOND PARAMETER* indicate if key exists on map
_, eJobs2 := salary["Jobs2"]
_, eBoss := salary["Boss"]
fmt.Printf("%f | exist? %v\n", salary["Jobs2"], eJobs2)
fmt.Printf("%f | exist? %v\n", salary["boss"], eBoss)
```
> EXT REF: zero-value for types:

|type|zero-value|
|---|---|
|numeric|0|
|bool|false|
|string|""|
|*pointer|nil|

## iteration
pretty much like iteration of Array (use range func)

with **FOR RANGE** flavor iteration, access order is **NOT** guaranteed

``` go
for key, value := range(salary) {
    fmt.Printf("Salary of %v is %v\n", key, value)
}
```

## delete
just use `delete(map, key)` func

``` go
delete(salary, "Boss") // how DARE you to delete boss from salary table!
```

## facts about Map

\* maps are **ref-type** instead of value-type

\* == can only used to compare a map and nil


# Pointer
## Why and when we need pointers
### Why
Everything in go is passed by value, use pointer
1. to make sure changes happened on original data object, not replica
2. to reduce memory consumption when interact with complex data structure

### When

## reference a variable (get pointer)

``` go
aNum := "123"
ref := &aNum
fmt.Printf("Type of ref is : %T\n", ref)
fmt.Println("Address of aNum is : ", ref)
```



## deference variable (get value of pointed)

``` go
aNum := "123"
ref := &aNum
*ref = "123 edited"
fmt.Printf("check value again: %v\n", aNum)
// or *CLAIM* a new space and change value inside
pString := new(string)
*pString = "Hello World"
fmt.Printf("check value: %v\n", *pString)
```

## facts about Pointer
\* avoid passing pointer of array into function, use slice instead (but **WHY**?)
\* zero value of `pointer` is `nil`

# Structure

> Structure just like interface in TS ...

## declaring

``` go
// named structure
type Employee struct {
    fstName string
    lastName string
    age int
}

// anonymous structures
var emp struct {
    fstName string
    lastName string
    age int
}
```

## creating

> Similar to `initialize a instance` ...

## zero value

zero value of structure is a instance of zero-value of all members

``` go
type Emp struct {
    name: string
    age: int
}

// zero value ↓
{ '', 0 }
```

## access property
``` go
emp3 := struct {
	name string
	age  int
}{
	name: "vljmr", age: 30,
}
fmt.Println(emp3.name)

```

## pointer of structure

``` go
	pEmp3 := &emp3
	// support both way
	fmt.Println(pEmp3.name)
    fmt.Println((*pEmp3).name)
```

both way to access field are supported, compiler thing ..

## nested structure
ofc, like a json

## promoted fields

``` go
type Address struct {  
    city, state string
}
type Person struct {  
    name string
    age  int
    Address
}

```

fields of Address can be access through Person, it's like some kind of **MERGE**

# Methods

method 也是 func, 但类似于 JS 中向原型添加方法

``` go
func (r Rectangle) Area() int {  
    return r.length * r.width
}
```

`(r Regtangle)` （在 Go 中成为接收器）标明 Area 方法可以定义在一个 Rectangle 结构上，r 即 Rectangle 实例

## WHY Method
1. Go 没有真正意义上的类，通过实现 type 上的 method 来实现像类一样的行为归并
2. 相同名字的 method 可以定义在不同的类型上，但如果通过 func 来做，没有办法实现相同签名、不同参数下的不同逻辑

## ==Pointer Receiver==

除了对可以对值类型定义 method 以外，还可以对指针定义 method

``` go
func (r *Rectangle) changeArea() int {  
    r.length = 100
    r.width  = 200
}
```

与定义在值类型上的 method 不同，定义在指针上的 method 对实例做的改变会反应回实例上

有时出于性能考虑的原因，为了避免包含大量属性的实例被拷贝传入 method, 我们会偏向于使用定义在指针类型上的 method 来避免参数拷贝

## method for non-local type

method receiver 只能定义在当前包内的类型

例如为 int 定义 method 则会收到报错

解决方法为：为 int 类型创建当前包的 alias

``` go
type myInt int
```

接下来在使用 int 的地方都使用 myInt 代替，有点像在 int 之上用管道过滤器封了一层，myInt 接管了 int 的行为，并提供新的方法定义

# Interface
在 go 中，使用接口本质上并不是像 Java 一样显式地定义和声明类实现了接口，而是通过定义类型的方式达到定义接口的目的

## 基本操作

``` go
// interface
type Animal interface {
    makeNoise()
}

// types (CLASS like)
type Cat {}

type Dog {}

// implementation
func (c Cat) makeNoise{
    fmt.Println("Meow")
}

func (c Dog) makeNoise{
    fmt.Println("Woof")
}

// 实际业务场景下使用统一定义的接口
func allMakeNoise(animals []Animal) {
	for _, v := range animals {
		v.makeNoise()
	}
}

```

除了 Cat / Dog 之外，还可以继续扩充新的实现 Animal 接口的动物类，而不需要改动 allMakeNoise 的业务代码

\* 空接口: `interface {}`

## 类型断言

``` go
func assert(i interface{}) {  
    s := i.(int) //获取实际的类型值
    fmt.Println(s)
}

func main () {
    var s interface{} = 56
    assert(s)
}
```

以上方法如果传入的不是显式调用的 int 值, 会导致 panic, 读取额外的状态 2 参数可以避免 panic

``` go
func assert(i interface{}) {  
    value, isOk := i.(int) // 额外读取 isOk 参数，如果不ok, value会被赋零值
    fmt.Println(s)
}
```

`i.(int)` / `i.(string)` 这样的用法还可以将实际类型参数化放到 switch 中使用

``` go
func findType(i interface{}) {  
    switch i.(type) {
    // 比较内建 type
    case string:
        fmt.Printf("I am a string and my value is %s\n", i.(string))
    case int:
        fmt.Printf("I am an int and my value is %d\n", i.(int))
    // Animal 接口也可以放到比较当中来
    case Animal:
        fmt.Printf("a Animal ...")
    default:
        fmt.Printf("Unknown type\n")
    }
}
```

## 实现多个接口
对同一个 type 定义多个 method 来实现各自 interface 要求的 Method 即可

## 嵌套接口

``` go
type SalaryCalculator interface {  
    DisplaySalary()
}

type LeaveCalculator interface {  
    CalculateLeavesLeft() int
}

type EmployeeOperations interface {  
    SalaryCalculator
    LeaveCalculator
}
```

## 零值

interface 的零值是 nil.


# 协程 (Goroutines)

协程可以看做一种轻量级的线程

协程有点像 js 的异步时分复用，但不一样的是，如果有类似等待用户输入的事情发生，不会像 js 一样移交控制权等待回调，而是开启新线程执行别的 goroutines

上千个协程可能只会复用几个线程，有时候甚至只会用一个

## 基本操作

``` go
func hello () {
    fmt.Println('Say hello in routine')
}

func main () {
    go hello()
    fmt.Println('in main')
}
```

在方法前加 go 关键字来使用协程调用方法，hello 在被调起来之后，main 方法中接下来一行代码继续获得自己独立的控制权，hello 的返回值会被无视

\* 主程 (main) 退出会导致其他所有协程都退出

# Channel

零值为 nil

Channel 的类型定下来之后，只能在 Channel 内传输符合类型定义的数据类型，不能传输其他类型

## init

``` go
channelX = make(chan bool)
```

初始化一个只能传输 bool 类型的 Channel

## 死锁 DeadLock

如果

1. 一个协程向 channel 写入，但没有任何一个地方读取
2. 一个地方读取了 channel, 但协程没有写入

会导致死锁

## 从 channel 不停读取数据
每次 channel 赋值都只会读取一次赋值的数据，如果 routine 不停在写数据，读取方需要循环来读取

``` go
func floodData(dataChan chan<- int) {
	for i := 1; i < 20; i++ {
		time.Sleep(1 * time.Second)
		// fmt.Printf("%c ", i)
		dataChan <- i
	}
	close(dataChan)
}

func main () {
    sendch := make(chan int)
	go floodData(sendch)
	for {
		v, ok := <-sendch
		if ok == false {
			fmt.Println("Closed")
			break
		} else {
			fmt.Printf("Recv: %v\n", v)
		}
	}
}
```

循环读取的 for 过程可以用 range 简化:

``` go
for v := range sendch {
    fmt.Printf("Recv: %v\n", v)
}
```

## channel 方向

channel 有方向性，通常我们使用 make(chan int) 构建一个双向 channel, 可以向这个 channel 写入数据，也可以用来读取数据

使用 make(chan<- int) 声明一个只能写入的 channel
\* 注: 通常应该是需要搭配双向 channel 类型转换使用，不然只能写入有什么用呢

双向 channel 在传入 goroutine 的时候可以转为单向 channel


## 关闭 channel

使用 close(xxChannel) 来关闭一个 channel

关闭后读取方会收到一个状态更新，状态使用第二个参数读取

``` go
v, ok := <-someCh
```

## WaitGroup

有点类似 JS 里的 Promise.all()

# Select

有点类似 JS 里的 Primise.race() ... 

# Mutex (临界区)

共享变量在存在竞态的时候成为临界区，在操作临界区的时候需要用 Mutex 先锁上，保证只有一个协程在访问, 操作完毕后又解锁

``` go
var mu sync.Mutex
mu.Lock()
x++
mu.Unlock()
```

# OOP
GO 没有真正意义上的 Class，只有 type

Go 通过一些其他手段实现类似 Class 的功能

如通过定义并暴露 `func New(params)` 来模拟 constructor

\* Go 没有继承，只有组合

## 多态

类型通过实现 Interface 中声明的所有方法，来隐式声明实现 Interface

纵然 Go 没有父子类，但也可以像 Java 一样 (参考 List 接口)，通过使用声明的接口来调用通用方法，达到多态的效果

# Defer

defer 用来将某个函数调用推迟到 当前上下文 return 之前

如 WaitGroup，如果存在多个 return 语句，需要保证每次 return 之前都调用 WaitGroup 的 Done 方法，否则会可能导致死锁，直接 defer Done 方法就可以只写一次该方法，并且能保证无论当前上下文以什么姿势 return, 该方法都会被调用

defer 的执行顺序是后进先出（栈结构）

# Panic

runtime 遇到 panic 的时候，会立即停止执行接下来的代码，将所有 defer 全部执行出栈，然后返回 e

从这个角度，panic 机制有点像 nodejs 的 process.exit(1)

## recover

recover 只能恢复当前协程里的 panic

# 函数第一公民

Go 中函数也是第一公民，可以：

1. 直接定义匿名函数并赋值给变量
2. 将函数作为参数传递
3. 函数声明完成后直接调用 (JS 里的立即执行函数表达式 IIFE)
4. 定义高阶函数

# 练手项目

> 读一千遍不如多做几遍，学自然语言还是学编程语言都需要不断练习，保持「语感」
