# Go



- 函数、函数签名、方法

  - 函数

    - 函数是执行特定任务的独立代码块。

    - 在Go语言中，函数可以定义在包级别，也就是说它不属于任何结构体、类型或对象。

    - 函数可以接收参数，并且可以返回结果。

    - 示例:

      ```go
      func Add(a int, b int) int {
          return a + b
      }
      ```
    
  - 函数签名（Function Signature）:

    - 函数签名定义了函数的名称、参数列表和返回值。
    - 它描述了函数的接口，即它接收什么参数，返回什么结果。
    - 例如，func Add(a int, b int) int的函数签名是Add(a int, b int) int，表示这是一个名为Add的函数，接收两个int类型的参数，并返回一个int类型的结果。

  - **方法（Method）**:

    - 方法是与特定类型（如结构体或接口）相关联的函数。

    - 在Go语言中，方法是定义在类型上的，并且可以访问该类型的数据。

    - **方法的定义包含一个"接收者"（receiver），它指定了该方法属于哪个类型。**

      ```go
      type Person struct {
          Name string
          Age  int
      }
      
      func (p Person) SayHello() {
          fmt.Println("Hello, my name is", p.Name)
      }
      //在上面的例子中，SayHello()是Person类型的一个方法。
      ```

- 包

  - **首字母大写可导出访问，适用于全局变量、全局常量、函数、类型、字段和方法等。**

  

## 一、基础概念

### 1、Go语言的核心特性：类型系统、接口、并发。
- Go语言为了解决当下编程语言对并发支持不友好，编译速度慢、编程复杂这三个问题而诞生的。
- 特征解读：
	- 函数以`func`为开头，函数体开头的`{`必须在函数头所在行尾部，不能单独起一行。
	- `main`函数所在报名必须是`main`

### 2、类型

#### 2.1 基本类型

- 变量定义

  - 用关键词`var`来定义变量

    - ```go
      var a int = 10
      var b = false //省略类型
      ```

  - 简短模式

    - (1) 显式初始化

    - (2) 不能提供类型

    - (3) 仅限函数内部

      ```go
      func main(){
        x := 10
      }
      ```

    - **退化赋值的前提条件：最少有一个新变量被定义，且在同一作用域**

      ```go
      func main(){
        x := 10
        x, y := 20, "abc"
      }
      ```

- 常量

  - **必须是编译期间可确定的量**；
  - 可指定类型，或用编译器推断；
  - 可在函数内定义；
  - 未使用常量不会引发编译错误。

- 枚举：没有明确的枚举类型（enum），一般借助`iota`实现

  - ```go
    const(
    	x = iota //x = 0
      y	//y = 1
      z	//z = 2
    )
    ```

- 基本类型

  - 进制转换

    - `math`：各数字的取值范围

    - `strconv`：进制或者字符串之间转换

      - ```go
        func main(){
          a, b, c := 0b1010, 0o144, 0x64
          fmt.Printf("0b%b, %#o, %#x\n",a ,b, c)
          fmt.Println(math.MinInt8)
        }
        ```

  - `nil`：

    - 默认是零值，而非空
    - 是预定义标识符，而不是关键字

- **别名**

  - ```go
    type X = int //别名
    ```

#### 2.2 引用类型

- 引用类型具有更复杂的内部结构

  - `new`：类型大小分配零值内存，返回指针。**只关心类型长度，不涉及内部结构和逻辑。优先在栈上分配。**

    - ```go
      func main(){
          s := *new([]int) //	slice:{ptr, len, cap}
          m := *new(map[string]int) //	map: ptr
          c := *new(chan int) //	chan: ptr
          
          p = new(int)
          *p = 100
      }
      ```

    - 

  - `make`：转为类型构造函数，完成内部初始。

    - ```go
      s := make([]int, 10, 100) //分配底层数组，并初始化len和cap字段
      s[0] = 1
      
      m := make(map[string]int, 10)
      m["a"] = 1
      ```

    - 

### 3、if语句
- `if`后面的条件判断句不需要括号括起来
- `{`必须放在行尾，和`if`或者`if else`放在一起
- `if`后面可以带一个简单的初始化语句，并且以分号分割，该简单语句声明的变量的作用域是整个`if`语句块，包括后面的`else if`和`else`分支
```Go
if x := f(); x < y{
  return x
}else if x > z{
  return z
}else{
  return y
}
```
### 4、switch语句

- ***

```Go
switch i := "y", i{
case "y", "Y":
	fmt.Println("Yes")
	fallthrough
case "n", "N"
	fmy.Println("No")
}
```

### 5、for循环

- 1、类似 C 里面的for循环语句
		`for init; condition; post{}`
- 2、类似 C 里面的 while 循环语句
		`for condition{}`
- 3、类似 C 里面的 while(1) 死循环语句
		`for {}`
- 4、对数组、切片、字符串、map和通道的访问，语法格式如下
	
	```go
	//访问map
	for key, value := range map{}
	for key := range map{}
	//访问数组
	for index, value := range array{}
	for index := range array{}
	for _, value := range array{}
	//访问切片
	for index, value := range slice{}
	for index := range slice{}
	for _, value := range slice{}
	//访问通道
	for value := range channel{}
	```

### 6、标签和跳转
- 标签：使用标签来标识一个语句的位置，用于`goto、break、continue`语句的跳转。
	
		Lable: Statement
	
- `goto`语句有以下几个特点：
	
	- `goto`语句只能在函数内跳转
	
	- **`goto`语句不能跳过内部变量的声明语句，例如**
		
	- ```go
		goto L //BAD，跳过v := 3 不被允许
			v := 3
			L:
		```
		
	- `goto`语句只能挑到同级作用域或者上层作用域内，不能跳到内部作用域内，例如：
		
		```go
  	if n % 2 == 1{
  		goto L1
  	}
		for n > 2{
			f()
	  	n--
	  L1:
	    f()
		  n--
		}
		```
	
- break：用于跳出for、switch、select等语句的执行
	- 单独使用，用于跳出当前所在的for、switch、select 语句的执行
	- 和标签一起使用，用于跳出标签所标识的for、swicth、select语句的执行，可用于跳出多重循环。
		```go
		L1:
			for i := 0; ; i++{
				for j := 0; ; j++{
          if i >= 5{
            //跳出L1标签所在的for循环
            break L1
          }
          if j > 10{
          	//跳出离break最近的内层循环
            break
          }
        }
			}
		```

- continue:

### 7、数据结构

#### 7.1 切片

- 切片构造方法

  - ```go
    func main(){
        array := [...]int{0,1,2,3,4,5,6,7,8}
        slice1 := array[2:6:8]
        
        slice2 := []int{1,2,3,4,5,6}
        
        slice3 := make([]int, 5)
        slice4 := make([]int, 0, 5)
    }
    ```

  - 可获取元素指针，但是不能以切片指针访问元素。

#### 7.2 字典

- 特性
  - 引用类型，无序键值对集合
  - 以初始化表达式或者是make函数创建
  - 可以判断字典是否为`nil`，不支持比较操作
  - 访问不存在主键，返回零值，推荐使用`x, ok := m["a"]`进行访问。
  - **迭代以随机次序返回**
  - **值不可寻址，需整体赋值**

## 二、函数
#### 实参到形参的传递
- go函数实参到形参的传递永远都是值拷贝，有时函数调用后实参指向的值发生了变化，那是因为参数传递的是指针值的拷贝，实参是一个指针变量，传递给形参的是这个指针变量的副本，二者指向同一地址，本质上参数传递仍然是值拷贝。

- 不定参数：不定参数声明使用`param ...type`的语法格式

  - 1、所有不定参数类型必须是相同的；

  - 2、不定参数必须是函数的最后一个参数；

  - 3、不定参数名在函数体内相当于切片，对切片的操作同样适合对不定参数的操作。例如：

    ```go
    func sum(arr ...int)(sum int){
      for _, v := range arr{
        sum += v
      }
      return
    }
    ```

  - 4、切片可以作为参数传递给不定参数，切片名后面要加上`...`，例如：

    ```go
    func sum(arr ...int)(sum int){
      for _, v := range arr{
        sum += v
      }
      return
    }
    func main(){
      slice := []int{1,2,3,4,5,6}
      array := [...]int{1,2,3,4,5,6,7}
    }
    ```
    
	- 5、形参为不定参数的函数和形参为切片的函数类型不同
	
	  ```go
	  func suma(arr ...int)(sum int){
	    for v := range arr{
	      sum += v
	    }
	    return
	  }
	  func sumb (arr []int) (sum int){
	    for v := range arr{
	      sum += v
	    }
	    return
	  }
	  ```
	
#### 函数签名和匿名函数

- 函数类型
		
	
		函数类型又叫函数签名，一个函数的类型就是函数定义首行去掉函数名、参数名和`{`，比如： func(int, int) int
	

	- 两个函数类型相同的条件是：拥有相同的形参列表和返回值列表（列表元素的次序、个数和类型都相同），形参名可以不同。
	- 可以使用type定义函数类型
	```go
	package	main
	import "fmt"
	
	func add(a, b int) int{
	  return a+b
	}
	func sub(a, b int) int{
	  retrun a-b
	}
	type Op func(int, int) int
	
	func do(f Op, a, b int) int {
	  return f(a, b)
	}
	func main(){
	  a := do(add, 1, 2)
	  b := do(sub, 1, 2)
	}
	```
	
	- 函数类型和map、 slice、 chan一样，实际函数类型变量和函数名都可以当作指针变量，指针指向个函数代码的开始位置。通常说函数类型变量是一种引用类型，为初始化的函数类型变量的默认值是nil。
	
	- Go中函数是”第一公民“，有名函数的函数名可以看作函数类型的常量，可以直接使用函数名调用函数，也可以直接复赋值给函数类型变量，后续通过该变量来调用函数
	
	  ```go
	  package main
	  func sum(a, b int) int {
	    return a+b
	  }
	  func main(){
	    sum(3, 4)//直接调用
	    f := sum //有名函数可以直接赋值给变量
	    f(1, 2)
	  }
	  ```

- 匿名函数

  - 匿名函数可以看作函数字面量，所有直接使用函数类型变量的地方都可以由匿名函数替代。匿名函数可以直接赋值给函数变量，可以当做实参，也可以作为返回值，也可以直接被调用。

    ```go
    package main
    
    import "fmt"
    //匿名函数被直接赋值函数变量
    var sum = func(a, b int) int{
      return a+b
    }
    
    func doinput(f func(int, int) int, a, b int) int {
      return f(a,b)
    }
    
    //匿名函数作为返回值
    func wrap(op string) func(int, int) int{
      switch op{
        case "add":
          return func(a, b int) int{
            return a+b
          }
        case "sub":
          return func(a, b int) int {
            return a-b
          }
        default:
        	retrun nil
      }
    }
    
    //匿名函数被直接调用
    ```

#### defer
- 注册多个延迟调用，这些调用以先进后出顺序在函数返回前被执行。defer常用于保证一些资源最终一定能够得到回收和释放；

  ```go
  package main
  
  func main(){
    //先进后出
    //定义匿名函数直接被调用
    defer func(){
      println("first")
    }()
    
    defer func(){
      println("second")
    }()
    
    println("function body")
  }
  
  //输出
  function body
  second
  first
  ```

  - defer后面必须是函数或方法的调用，不能是语句，否则会报错；

  - defer函数的实参在注册时通过值拷贝传递进去，后期变量被修改不影响注册进入的值；

    ```go
    func f() int{
      a := 0
      defer func(i int){
        println("defer i=", i)
      }(a)
      a++
      return a
    }
    ```

  - defer必须先注册再执行，例如不能放到return语句后面；
  - 主动调用os.Exit(int)退出进程，deser将不再被执行（即使defer被提前注册)；
  - 使用defer可以避免资源泄露；
  - defer会推迟资源的释放，并且defer尽量不要放到循环语句里面；
  - defer最好不要对有名返回值参数进行操作；

#### 闭包

- 闭包是由函数及其相关引用环境组合而成的实体，一般通过匿名函数中引用外部函数的局部变量或包全局变量。	闭包 = 函数 + 引用环境 。
  - 如果函数返回的闭包引用该函数的局部变量（函数参数或者是函数内部变量）：
    - 1、多次调用函数，返回的多个闭包所引用的外部变量是多个副本，原因是每次调用函数都会为局部变量分配内存。**（针对返回闭包函数）**
    
    - 2、用一个闭包函数多次，如果该闭包修改了其引用的外部变量，则每一次调用该闭包对该外部变量都会有影响，因为闭包函数共享外部引用。**（针对闭包函数）**
    
      ```go
      package main
      func fa(a int) func(c int) int{
        return func(i int) int{
          println(&a, a)
          a = a + i
          return a
        }
      }
      func main(){
        f := fa(1)
        g := fa(1)
        // 此时f，g引用的闭包环境中的a的值不是同一个，而是两次函数调用产生的副本
        println(f(1))
        println(f(1))
        
        println(g(1))
        println(g(1))
      }
      //程序运行结果
      0xc4200140b8 1
      2
      0xc4200140b8 2
      3
      0xc4200140c0 1
      2
      0xc4200140c0 2
      3
      ```
    
      - 注意返回的闭包函数其实是不同的副本，引用的变量副本不同，相应的地址也不同。但是针对同一个闭包函数，共享内部引用的变量，对其修改，会影响后续的值。
      
      - 如果一个函数调用返回的闭包引用修改了全局变量，则每次调用都会修改全局变量。
      
      - 如果函数返回的闭包引用的是全局变量a，则多次调用该函数返回的多个闭包引用的都是同一个a。同理，调用一个闭包多次引用的也是同一个a。**使用闭包是为了减少全局变量，所以闭包引用全局变量不是很好的编程方式**
      
        ```go
        package main
        
        var (
        	a = 0
        )
        func fa() func(i int) int {
          return func(i int) int {
            println(&a, a)
            a = a + i
            return a
          }
        }
        func main(){
          f := fa()
          g := fa()
          //f, g引用的外部变量是同一个a
          
          println(f(1))
          println(g(1))
          println(g(1))
          println(g(1))
        }
        //程序调用结果
        0x4c9868 0
        1
        0x4c9868 1
        2
        0x4c9868 2
        3
        0x4c9868 3
        4
        ```
      
        - 同一个函数返回的多个闭包共享该函数的局部变量。例如
      
          ```go
          package main
          func fa(base int) (func(int) int, func(int) int){
            println(&base, base)
            add := func(i int) int{
              base += i
              println(&base, base)
              return base
            }
            sub := func(i int) int{
              base -= i
              println(&base, base)
              return base
            }
            return add, sub
          }
          func main(){
            //f, g闭包引用的base是同一个
            f, g := fa(0)
            
            s, k := fa(0)
            
            println(f(1), g(2))
            println(s(1), k(2))
          }
          
          0xc4200140b8 0
          0xc4200140c0 0
          0xc4200140b8 1
          0xc4200140b8 -1
          1 -1
          0xc4200140c0 1
          0xc4200140c0 -1
          1 -1
          
          ```

- 闭包的作用

		闭包最初的目的是减少全局变量。对象是附有行为的数据，而闭包是附有数据的行为。

#### panic 和 recover
- panic用来主动抛出错误，recover用来捕获panic抛出的错误。
- 发生panic后，程序会从调用panic的函数位置或发生panic的地方立即返回，逐层向上执行函数的defer语句，并且逐层打印函数调用堆栈，直到被recover捕获或运行到最外层函数退出。
- recover()用来捕获panic，阻止panic继续向上传递。recover()和defer一起使用，但是只有在defer后面的函数体内被直接调用才能捕获panic终止异常，否则返回nail.
- `recover`只能在延迟调用内正确执行，直接注册为延迟调用或者被延迟函数间接调用都无法捕获`panic`。

```go
//这个会捕获失败
defer revover()
//这个会捕获失败
defer fmt.Println(recover())

//这个嵌套两层函数也会捕获失败
defer func(){
  func(){
    println("defer inner")
    recover()//无效
  }()
}()

//捕获成功
defer func(){
  println("defer inner")
  recocer()
}()

func except()
{
  recover()
}

func test(){
  defer except()
  panic("test panic")
}
```

- 延迟场景中，可以有多个panic报错，但是只有最后一个才会被捕获；

- init函数引发的panic只能在init函数中被捕获，原因是init先于main执行。函数不能捕获内部新启动的goroutine所跑出的panic；

### 错误处理

- 错误和异常

  - 狭义的错误：发生非期望的已知行为，这里的已知是指错误的类型是预料并定义好的。

  - 异常：发生非期待的未知行为。异常又被称为未捕获的错误。比如C中的段错误。

  - 错误可以分为两类：

    - 运行时错误，可以被捕获，并采取措施——隐式抛出panic

    - 程序逻辑错误

## 三、类型系统

### 1、命名类型和未命名类型

- 命名类型：命名类型可以通过标识符来表示，包括自定义类型。

- 未命名类型：一个类型由预声明类型、关键字和操作符组合。复合类型：数组、切片、字典、通道、指针、函数字面量、结构和接口。其中`*int`, `[]int`, `[2]int`, `map[k]v`

  - **类型字面量：**类型字面量不是使用预先定义的类型名称，而是直接在声明或者使用的地方定义了类型的结构。这通常用于创建匿名结构体、匿名接口、切片类型、映射（map）类型、通道（channel）类型等。

  ```go
  package main
  import "fmt"
  
  //使用type声明的事命名类型
  type Person struct{
    name string
    age int
  }
  //匿名结构体
  var person = struct {
      Name string
      Age  int
  }{
      Name: "Alice",
      Age:  30,
  }
  //匿名接口
  func process(input interface {
      DoSomething()
  }) {
      input.DoSomething()
  }
  //切片类型
  var numbers = []int{1, 2, 3}
  
  
  func main(){
    //使用struct字面量声明的是未命名类型，匿名结构体
    a := struct{
      name string
      age int
    }{"andes", 18}
    
    fmt.Printf("%T\n", a)
    fmt.Printf("%v\n", a)
    
    b := Person{"tom", 21}
    fmt.Printf("%T\n", b)
    fmt.Printf("%v\n", b)
  }
  ```

  - 具有相同声明的未命名类型被视作同一类型。

    ```go
    func main(){
        [2]int, [3]int //未命名类型：长度不同，不是同一类型。
        []int, []byte //元素类型不同
        
        var a, b interface{
            test()
        }
        println(a == b)
    }
    ```

- 底层类型

  - (1) 预声明类型和类型字面量的底层类型是自身
  - (2) 自定义类型的底层类型是逐层递归向下查找的，直到查找的oldtype是预声明类型或者是类型字面量

- 类型相同和类型赋值

  - 类型相同

    - 两个命名类型相同的条件是两个类型声明的语句完全相同。
    - 命名类型和未命名类型完全不相同
    - 两个未命名类型相同的条件是它们的类型声明字面量的结构相同，并且内部元素的类型相同
    - 通过类型别名语句声明的两个类型相同
    - **类型别名**: `type T1 = T2`

  - **类型赋值**

    - 不同类型之间的变量之间一般不能直接相互赋值

    - a可以赋值给变量b必须要满足如下条件中的一个

      - 1、T1 和 T2的类型相同

      - 2、**T1 和 T2 具有相同的底层类型，并且T1 和 T2中至少有一个是未命名类型**

      - 3、**T2是接口类型，T1是具体类型，T1的方法集是T2方法集的超集**

      - 4、T1 和 T2 都是通道类型，拥有相同的元素类型，并且T1 和 T2中至少有一个是未命名类型。

      - 5、a是预声明标识符nail， T2是`pointer、function、slice、map、channel、interface等类型`

      - 6、a是一个字面常量值

        ```go
        package main
        
        import (
        	"fmt"
        )
        
        type Map map[string]string
        func (m Map) Print(){
          for _, key := range m{
            fmt.Println(key)
          }
        }
        type iMap Map
        
        func (m iMap) Print(){
          for _, key := range m{
            fmt.Println(key)
          }
        }
        
        type slice []int
        func (s slice) Print(){
          for _, v := range s{
            fmt.Println(v)
          }
        }
        
        func main(){
          mp := make(map[string]string, 10)
          mp["hi"] = "tata"
          //mp 和 ma具有相同的底层类型，且mp是未命名类型
          var ma Map = mp
          //虽然具有相同的底层，但是都不是未命名类型，不能赋值
          var ma iMap = ma//无法编译
          //强转
          var im iMap = iMap(ma)
        }
        ```

- 类型强转

  - 非常量类型的变量x可以强转需要满足以下条件

    - (1)、x可以直接赋值给T变量

    - (2)、x的类型和T具有相同的底层类型

    - (3)、x的类型和T都是未命名的指针类型，并且指针指向的类型具有相同的底层类型

    - (4)、x的类型和T都是整型，或者都是浮点类型

    - (5)、x的类型和T都是复数类型

    - (6)、x是整数型类型或[]byte或者是[]rune,T是string类型

    - (7)、x是字符串，T是[]byte或者是[]rune

      ```go
      s := "hello, world"
      var a []byte
      a = []byte(s)
      
      var b string
      b = string(a)
      
      var c []rune
      c = []rune(s)
      
      ```


### 2、类型方法
#### 2.1 自定义类型

	自定义类型使用关键字type，语法格式`type newtype oldtype`。`oldtype`可以是自定义类型、预声明类型、未命名类型中的任意一种，其中`newtype`与`oldttype`具有相同的底层类型，并且都继承了底层类型的操作。**自定义类型都是命名类型**。
```go
type INT int
type Map map[string]string
type myMap Map //myMap是一个自定义类型Map声明的自定义类型
```

#### 2.2 自定义struct类型

  - struct初始化

    ```go
    type Person struct{
      name string
      age int
    }
    ```

    - (1) 按照字段顺序进行初始化

      ```go
      a := Person("andes", 18)
      b := Person{
        "andes",
        18,
      }
      c := Person{
        "andes",
        18
      }
      ```
    - (2) 指定字段名初始化，推荐该方法，因为struct增加字段，不用修改初始化语句

      ```go
      a := Person{name: "andes", age: 18}
      b := Person{
        name: "andes",
        age: 18,
      }
      c := Person{
        name: "andes"
        age: 18
      }
      ```

    - (3) 使用new创建内置函数，字段默认初始化其类型的零值，返回值是指向结构的指针。例如：
    ```go
    p :=new{Person}
    ```
    - (4) 一次初始化一个字段
    - (5) 使用构造函数进行初始化
    ```go
    func New(text string) error{
      return &errorString{text}
    }
    type errorString struct{
      s string
    }
    ```

#### 2.3 匿名

- 隐式以类型名为字段名

- 嵌入类型与其指针类型隐式字段名相同

- 可以像直属字段访问嵌入类型成员

  - 存在重名情况：编译器优先直属命名字段，或按嵌入层次逐级查找匿名类型成员

  - ```go
    type File struct{
        name []byte
    }
    type Data struct{
        File
        name string
    }
    
    func main(){
        d := Data{
            name : "data"
            File: File{[]byte("file")}
        }
        d.name = "data2" //优先直属命名字段
        d.File.name = []byte("file")
        
    }
    ```

  - 


#### 2.4 结构字段的特点
  - 可以是任意的类型，基本类型、接口类型、指针类型、函数类型等。结构字段的类型名必须唯一，结构体自身可以嵌套自身的指针，以实现数、链表等数据结构。
  - 匿名字段：定义struct的过程中，如果字段只给出字段类型，没有给出字段名，则称这样的字段为匿名字段。

  ```go
  type File struct{
    *file
  }
  ```

- 自定义接口类型

  ```go
  //interface{}是接口字面量类型，所以i事非命名类型
  var i interface{}
  //Reader 是自定义接口类型，属于命名类型
  type Reader interface{
    Read(p []byte)(n int, err error)
  }
  ```

### 3、方法

-  go中类型方法是一种对类型行为的封装。Go语言的方法可以看作特殊类型的函数，其显式地将对象实例或指针作为函数的第一个参数，并且函数名可以指定。这个对象实例或者指针被称为`reciever`

  - 示例：

    - ```go
      func (t TypeName) MethodName (paramList)(Returnlist){
        //...
      }
      
      func (t *TypeName) MethodName (paramList)(Returnlist){
        //...
      }
      ```

    - t是接收者，可以自由指定名称

  - 类型方法本质是一个函数，将类型方法改写为普通的函数，示例：

    - ```go
      func TypeName_MethodName (t TypeName, OtherparamList)(Returnlist){
        //...
      }
      //示例
      type SliceInt []int
      
      func (s SliceInt) Sum() int{
        sum = 0
        for _, i := range s{
          sum += i
        }
        return sum
      }
      
      //该函数与上面的类型方法等价
      func SilceInt_Sum(s SliceInt) int{
        sum := 0
        for _, i := range s{
            sum += i
        }
        return sum
      }
      ```

    - 类型方法的特点：

      - 可以为命名类型增加方法，非命名类型不能自定义方法，如`[]int`非命名类型。

      - 方法的定义与类型的定义必须在同一个包中，比如不能为预声明类型；`int`, `bool`类型增加方法。

      - 方法的命名空间的可见性和变量一样，大写开头的方法可以在包外面进行访问，否则包内可见。

      - 使用type定义的自定义类型是作为一个新类型，新类型不能调用原来类型的方法，但是底层类型支持的运算(range, +-*/)可以被新类型继承。

        - ```go
          type Map map[string]string
          func (m Map) Print(){
            for_, key := range m{
              fmt.Pringln(key)
            }
          }
          ```

- **如何确定接受参数类型**
  - 修改实例状态，用`*T`；
  - 不修改状态的小对象或固定值，用`T`；
  - 大对象用`*T`，减少复制成本；
  - 引用类型、字符串、函数等指针包装对象，用`T`；
  - 含`Mutex`等同步字段，使用`*T`，避免因为复制而造成锁失效；
  - 其他无法确定的使用`*T`。

#### 方法调用

- 一般调用： `TypeInstanceName.MethodName(paramList)	`

#### 方法值

- 带有闭包的函数变量，其底层实现原理和带有闭包的匿名函数类似。变量x的静态类型是T，m是类型T的一个方法，则`x.m`被称为方法值

 ```go
  typen T struct{
  	a int
  }
  func (t T) Get() int{
  	return t.a
  }
  
  func (t *T) Set(i int){
    t.a = i
  }
  func (t *T) Print(){
    fmt.Printf(...)
  }
  
  var t = &T{}
  
  f := t.Set
  f(2)
  
  t.Print()
  
  f(3)
 ```

#### 方法表达式

**将类型方法显式地转换为函数调用，接收者必须显式地传递进去。**

```go
type T struct{
  a int
}
func (t *T) Set(i int){
  t.a = i
}
func (t T) Get() int{
 	return t.a
}
func (t *T) Print(){
  fmt.Printf(...)
}
```

- `T.Get和*T.Set`是方法表达式，方法表达式可以看作为函数名，只不过这个函数的首个参数是示例或者是指针。**将方法还原为普通函数，显式传递接收参数。**

  - ```go
    t := T{a:1}
    //普通方法
    t.Get()
    //方法表达式
    (T).Get(t)
    
    f1 := T.Get; f1(t)
    
    f2 := (T).Get; f2(t)
    
    (*T).Set(&t, 1)
    f3 := (*T).Set; f3(&t, 1)
    
    
    ```

- **方法集**

  - 命名类型方法接收者有两种类型，一个是值类型，还有一个是指针类型，但是无论接收者是什么类型，方法和函数传递都是值拷贝。

    - ```go
      package main
      import "fmt"
      
      type Int int
      
      func (a Int) Max(b Int) Int{
        if a > b{
          return a
        }else{
          return b
        }
      }
      
      func (i *Int) Set(a Int){
        *i = a
      }
      func (i Int) Print(){
        fmt.Printf("value = %d\n", i)
      }
      func main(){
        var a Int = 10
        var b Int = 20
        c := a.Max(b)
        c.Print()
        (&c).Print()//编译器转换为c.Print()
        
        a.Set(20) //编译器转换为(&a).Set(20)
      }
      ```

    - （1）通过类型字面量显式地进行值调用和表达式调用，在这种情况下编译器不会进行自动转换，会进行严格的方法集检查

    - （2）表达式调用不会进行类型转换，方法值调用会进行自动转换

#### 组合和方法集
- 组合
	- 内嵌字段的初始化和访问
	  - 内嵌字段唯一，不需要全路径进行访问。
	  - 不同嵌套层次具有相同的字段时，应该使用全路径访问，以免使用过程中出现歧义。
	
	- 内嵌字段的方法调用

- **组合的方法集**
  - （1）如果类型S包含匿名字段T，则S的方法集包含T方法集。
  - （2）如果类型S包含匿名字段*T，则S的方法集包含T方法集和 *T方法集。
  - （3）不管类型S嵌入的匿名字段时T还是 \*T，\*S方法集总是包含T和\*T方法集。



#### 函数类型

- 函数类型分为两种：函数字面量类型（未命名类型），另外一种则是函数命名类型。、

  - 函数字面量类型：`func (InputTypeList)OurputTypelist`，可以看出，有名函数和匿名函数都属于函数字面量类型。有名函数是初始化一个函数字面量并且将其赋值给一个函数名变量，匿名函数的定义也是直接初始化一个函数字面量，没有绑定到一个具体变量上。

  - 函数命名类型：`type NewFuncType FuncListeral`

  - 函数签名：有名函数或者匿名函数的字面量类型，函数签名时函数的字面量类型，不包括函数名

  - 函数声明：Go调用Go编写的函数不需要声明，可以直接调用。调用汇编语言编写的函数需要进行声明。

    - ```go
      //函数声明 = 函数名 + 函数签名
      //函数签名
      func (InputTypeList) OurputTypeList
      
      //函数声明
      func FuncName (InputTypeList) OurputTypeList
      ```

  - 函数类型的定义

  
## 四、接口

- 接口是一个编程规范，也是一组方法签名的集合。接口只有只有声明没有实现，所以定义一个新接口，通常又被称为声明一个新的接口，定义接口和声明接口二者通用，代表相同的意思。

- 空接口：`interface{}`,任何类型的实例都可以赋值或传递给空接口。



#### 基本概念 
- 接口声明
  - 接口字面量类型的声明
   ```go
    interface{
    	MethodSignature1
   	MethodSignature2
    }
   ```
	- 接口命名类型
	```go
  type InterfaceName interface{
    MethodSignature1
    MethodSignature2
  }
  ```

  - **使用接口字面量的场景比较少，一般只有空接口`interface{}`类型变量的声明才会使用**
  
  - 接口定义内，可以是方法声明的集合，也可以嵌入另外一个接口类型匿名字段，也可以是两者的混合
  
  
  ```go
  type Reader interface{
    Read(p []byte) (n int, err error)
  }
  
  type Writer interface{
    Write(p []byte) (n int, err error)
  }
  
  type ReadWriter interface{
    Reader
    Writer
  }
  
  type Reader interface{
    Read(p []byte) (n int, err error)
    Writer
  }
  ```
  
  - 方法声明
  
  
  ```go
  //方法声明 = 方法名 + 方法签名
  MethodName (InputTypeList)OurputTypeList
  ```
  
  - 声明新接口类型的特点
    - （1）接口的命名一般以`er`结尾；
    - （2）接口定义的内部方法声明不需要`func`引导；
    - （3）在接口定义中，只有方法的声明没有方法的实现。
  
- 接口初始化

  - 接口变量支持两种初始化方法
    - 实例赋值接口
    - 接口变量赋值接口变量


- 接口方法调用
- 接口的动态特性和静态特性
  - 动态特性：接口绑定的具体实例的类型称为接口的动态类型。接口可以绑定不同类型的实例，所以接口的动态类型是随着其绑定的不同类型实例而发生的。
  - 静态特性：接口被定义时， 其类型就已经被确定，这个类型叫接口的静态类型。接口的静态类型在其定义时就被确定，静态类型的本质特征就是接口的方法签名集合。**如果两个皆苦的方法签名集合相同（顺序可以不同），则这两个接口子啊语义上完全等价，它们之间不需要强制类型转换就可以相互赋值。**

#### 接口运算
- 类型断言 `i.(TypeName)`

  - 如果`TypeName`是具体类型名
  - 如果`Typename`是接口类型名

  ```go
  package main
  
  import "fmt"
  
  type Inter interface{
    Ping()
    Pang()
  }
  type Anter interface{
    Inter
    String()
  }
  
  type St struct{
    Name string
  }
  
  func (St) Ping(){
    println("Ping")
  }
  func (*St) Pang(){
    Println("Pang")
  }
  
  func main(){
    st := &St("andes")
    var i interface{} = st
    //判断 i 不定的实例是否实现了接口类型Inter
    O := i.(Inter)
    O.Ping()
    O.Pang()
    
    //如下语句会引发panic，因为没有实现 Anter的 String()
    //p := i.(Anter)
   	//p.String()
    
    //判断i不定的实例是否就是具体类型St
    
    s := i.(St)
    fmt.Printf("%s", s.Name)
  }
  ```

  - `comma, ok`表达式模式如下

  ```go
  if o. ok := i.(TypeName); ok{
    
  }
  ```

- 类型查询

  - 接口类型查询的语法格式如下：

  ```go
  switch v := i.(type){
    case type1:
    	xxxx
    case type2:
    	xxxx
    default:
    	xxxx
  }
  ```

  - 接口查询有两层语义：第一层是查询一个接口变量底层绑定的底层变量的具体类型是什么，二是查询接口变量绑定的底层变量是否还实现了其他的接口。
    - i必须是接口类型：具体类型实例的类型是静态的，在类型声明后就不再变化，**所以具体类型的变量不存在类型查询，类型查询一定是对一个接口变量进行操作**。
    - case字句后面可以跟非接口类型名，也可以跟接口类型名

- 接口优点和实用形式

  - 使用形式

    - 1、作为结构内嵌字段

    - 2、作为函数或方法的形参

    - 3、作为函数或者方法的返回值

    - 4、作为其他接口定义的嵌入字段

      
#### 空接口

- 空接口表示为`interface{}`，系统中任何类型都符合空接口的要求。类似C++里面的`void*`
- **如果一个函数需要接收任意类型参数，可以使用空接口类型**

```go
package main

import "fmt"

type Inter interface{
  Ping()
  Pang()
}

type St struct{}

func(St) Ping(){
  println("Ping")
}

func(St) Pang(){
  println("Pang")
}

func main(){
  var st *St = nil
  var it interface = st
  fmt.Printf("%p\n", st)
  fmt.Printf("%p\n", st)
  if it != nil{
    it.Pang()
    it.Ping()
  }
  
}

```

#### 空接口的内部实现

## 五、并发

#### 并发基础

- 并发和并行

  - 并发：程序在单位时间内是同时运行的。
  - 并行：程序在在任意时刻都是同时运行的。

  - 并发注重在避免阻塞，使程序不会因为一个阻塞而停止处理。并发典型的应用场景：分时操作系统就是一种并发设计。

- goroutine：在用户层构建的一层调度，将并发的粒度进一步降低。

- 启动方式1： **go+匿名函数**

```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
	go func() {
		sum := 0
		for i := 0; i < 1000; i++ {
			sum += i
		}
		fmt.Println(sum)
		time.Sleep(1 * time.Second)
	}()
	fmt.Println("NumGoroutine = ", runtime.NumGoroutine())

	time.Sleep(5 * time.Second)
}

```

- 启动方式2：**go+函数名**

```go
package main

import(
	"runtime"
  "time"
  "fmt"
)
func sum(){
  sum := 0
  for i := 0; i < 10000; i++{
    sum += i
  }
  println(sum)
  time.Sleep(1 * time.Second)
}

func main(){
  go sum
  println("NumGoroutine", runtime.NumGoroutine())
  
  time.Sleep(5 * time.Second)
}
```

- **协程的特性**

  - 【1】go的执行是非阻塞的，不会等待。
  - 【2】go后面的函数的返回值会被忽略。
  - 【3】调度器不能保证多个`goroutine`的执行次序。
  - 【4】没有父子`goroutine`的概念，所有的协程都是平等地被调度和执行的。
  - 【5】go在执行时会单独为`main`函数创建一个`goroutine`，遇到其他go关键词再去创建其他的`goroutine`
  - 【6】go没有暴露`goroutine id`给用户，所以不能在一个线程里面显示的操作另外一个协程。

- `func GOMAXPROCS`：`func GOMAXPROCS(n int)int`用来设置或查询可以并发执行的`goroutine`数目，n大于1表示设置的值，否则表示查询当前的值

  ```go
  package main
  
  import "runtime"
  
  func main(){
  	//表示获取当前的GOMAXPROCS值
  	println("GOMAXPROCS = ", runtime.GOMAXPROCS(0))
  	
  	//表示设置GOMAXPROCS的值为2
  	runtime.GOMAXPROCS(2)
  	
  	//表示获取当前的GOMAXPROCS的值
  	println("GOMAXPROCS = ", runtime.GOMAXPROCS(0))
  }
  ```

- `func Goexit`：结束当前协程的运行，并且会执行当前协程的`defer`，不会产生`panic`
- `func Gosched`：放弃当前调度执行的机会，将当前协程放到队列中等待下次调度。



- **chan**：是协程之间通信和同步的重要组件。“不是通过共享内存来通信，而是用通信来实现共享内存”，通道`chan`是go通过通信来实现共享内存的载体。通道是有类型的管道，声明格式为`chan dataType`，实际使用`make`进行通道的创建。

```go
//创建一个无缓冲的通道，通道存放元素的类型为datatype
make(chan datatype)

//创建一个有10个缓冲的通道，元素类型为datatype
make(chan datatype, 10)
```

​		无缓冲的通道可以用于通信，也可以用于两个协程之间的同步，有缓冲的通道主要用于通信。

```go
package main

import (
	"runtime"
)

func main() {
	c := make(chan struct{})
	go func(i chan struct{}) {
		sum := 0
		for i := 0; i < 10000; i++ {
			sum += i
		}
		println(sum)
		c <- struct{}{}
	}(c)

	println("NumGoroutine", runtime.NumGoroutine())

	<-c
}

```

当协程运行结束退出后，写到缓冲通道的数据不会消失，可以缓冲和适配两个协程速率不一致的情况，缓冲通道和消息队列类似，具有削峰和增大吞吐量的功能。

```go
package main

import (
	"runtime"
)

func main() {
	c := make(chan struct{})
	ci := make(chan int, 100)

	go func(i chan struct{}, j chan int) {
		for i := 0; i < 10; i++ {
			ci <- i
		}
		close(ci)
		c <- struct{}{}
	}(c, ci)
	println("NumGoroutine =", runtime.NumGoroutine())
	//读通道
	<-c

	println("NumGoroutine =", runtime.NumGoroutine())
  //ci已经退出，但是可以继续读取
	for v := range ci {
		println(v)
	}
}

```

1、**可能造成`panic`的原因**

【1】向已经关闭通道写数据会导致`panic`；

【2】重复关闭的通道会导致`panic`

2、阻塞

【1】向未初始化的通道写入数据或读数据都会导致当前的协程永久阻塞；

【2】向缓冲区已满的通道写入数据会导致协程的阻塞；

【3】通道中没有数据，读取该通道会导致协程阻塞。

3、非阻塞

【1】读取已经关闭的通道不会引发阻塞，而是立即返回通道元素类型的零值，可以使用`comma, ok`语法判断通道是否已经关闭。

【2】向有缓冲区且没有满的通道中进行读写

- **WaitGroup**：通过`WaitGroup`实现多个协程同步的机制

​	主要数据结构如和操作如下：

```go
type WaitGroup struct{
  // contains filtered or unexported fields
}

//添加等待信号
func(wg *WaitGroup) Add(delta int)

//释放等待信号
func(wg *WaitGroup) Done()

//等待
func(wg *WaitGroup) Wait()
```

`WaitGroup`用来等待多个协程完成，`main goroutine`调用`Add`设置需要等待协程的数目，每一个协程结束时调用`Done`，`Wait()`被用来等待所有的协程完成。

​	【1】`wg.Add()`:用来设置一个计数器，表示需要等待完成的协程数量。

​	【2】`wg.Done()`来减少等待计数。`wg.Done()`实际上是`wg.Add(-1)`的简写。

​	【3】主协程中，你可以使用`wg.Wait()`来阻塞，直到所有的协程完成（即计数器减为0）。

```go
package main

import (
	"net/http"
	"sync"
)

var wg sync.WaitGroup
var urls = []string{
	"http://www.golang.org/",
	"http://www.google.com/",
	"http://www.qq.com",
}

func main() {
	for _, url := range urls {
		//每一个URL启动一个协程，同时需要给wg加1
		wg.Add(1)

		go func(url string) {
			//当前协程结束后给wg-1，wg.Done()等价于wg.Add(-1)
			//defer wg.Add(-1)
			defer wg.Done()

			//发送HTTP get请求并且打印HTTP返回码
			resp, err := http.Get(url)
			if err == nil {
				println(resp.Status)
			}

		}(url)
	}
	//等待所以请求结束
	wg.Wait()
}

```

- `select`：多路复用机制，用于多路监听多个通道，当监听的通道没有状态时可读或者可写的，那么select时阻塞的。如果监听的通道有多个可读或可写的状态，则随机选择一个进行处理。

基本用法：`select`语句用于同时等待多个通道操作。它类似于switch语句，但是每个case都是一个通道操作（发送或接收）。**`select`会阻塞，直到其中一个通道操作可以进行，然后它执行那个操作。**如果多个通道操作都准备好了，`select`会随机选择一个执行。

```go
select {
case x := <-chan1:
    // 如果chan1准备好读取，执行这里
    fmt.Println("Received", x, "from chan1")
case chan2 <- y:
    // 如果chan2准备好写入，执行这里
    fmt.Println("Sent", y, "to chan2")
default:
    // 如果上面都没有准备好，执行这里
    fmt.Println("No channel ready")
}

```

​	使用`select`来处理超时:`select`经常与`time.After`函数结合使用，以实现超时控制。

```go
select {
case res := <-c:
    fmt.Println(res)
case <-time.After(3 * time.Second):
    fmt.Println("Timeout")
}

```

​	使用`select`进行退出控制:可以使用一个专门的退出通道来通知协程退出：

```go
for {
    select {
    case <-stopCh:
        // 收到退出信号，退出循环
        return
    case <-ticker.C:
        // 定期执行的操作
    }
}

```

- 扇入和扇出
  - 扇入：通过`select`聚合多条通道服务到一条通道中处理；
  - 扇出：通过`go`启动协程并发处理，即将一条通道发散到多条通道中处理；
- **通知退出机制**

​		读取已经关闭的通道不会引起阻塞，也不会导致`panic`，而是立即返回该通道存储类型的零值。**关闭`select`监听的某个通道，并且使`select`能够立刻感知，然后进行相应的处理，这就是所谓的退出通知机制。利用`context`库进行操作**

​		示例随机数生成器，下游消费者不需要随机数时，显示地通知生产者停止生产：

```go
package main

import (
	"fmt"
	"math/rand"
	"runtime"
)

func GenerateIntA(done chan struct{}) chan int {
	ch := make(chan int)
	go func() {
	Lable:
		for {
			select {
			case ch <- rand.Int():
			case <-done:
				break Lable
			}
		}
		close(ch)
	}()
	return ch
}
func main() {
	done := make(chan struct{})
	ch := GenerateIntA(done)

	fmt.Println(<-ch)
	fmt.Println(<-ch)

	close(done)
	fmt.Println(<-ch)
	fmt.Println(<-ch)

	println("NumGoroutine = ", runtime.NumGoroutine())

}

```

#### 并发范式

- 生成器：例如统一生成全局事务号、订单号、序列号和随机数等。

  - 最简单的带缓冲的生成器

  ```go
  package main
  
  import (
  	"fmt"
  	"math/rand"
  )
  
  func GernerateIntA() chan int {
  	ch := make(chan int, 100)
  	go func() {
  		for {
  			ch <- rand.Int()
  		}
  	}()
  	return ch
  }
  
  func main() {
  	ch := GernerateIntA()
  	fmt.Println(<-ch)
  	fmt.Println(<-ch)
  	
  }
  ```

  - 多个协程增强型生成器

  ```go
  package main
  
  import (
  	"fmt"
  	"math/rand"
  )
  
  func GernerateIntA() chan int {
  	ch := make(chan int, 10)
  	go func() {
  		for {
  			ch <- rand.Int()
  		}
  	}()
  	return ch
  }
  
  func GernerateIntB() chan int {
  	ch := make(chan int, 10)
  	go func() {
  		for {
  			ch <- rand.Int()
  		}
  	}()
  	return ch
  }
  
  func GernerateInt() chan int {
  	ch := make(chan int, 10)
  
  	go func() {
  		//使用select的扇入技术增加生成的随机源
  		for {
  			select {
  			case ch <- <-GernerateIntA():
  			case ch <- <-GernerateIntB():
  			}
  		}
  
  	}()
  	return ch
  
  }
  func main() {
  	ch := GernerateInt()
  	for i := 0; i < 100; i++ {
  		fmt.Println(<-ch)
  	}
  }
    
  ```

  - 生成器自动退出，借助go的通知机制

  ```go
  package main
  
  import (
  	"fmt"
  	"math/rand"
  )
  
  func GernerateIntA(done chan struct{}) chan int {
  	ch := make(chan int)
  	go func() {
  	Lable:
  		for {
  			select {
  			case ch <- rand.Int():
  			case <-done:
  				break Lable
  			}
  		}
  		close(ch)
  	}()
  	return ch
  }
  
  func main() {
  	done := make(chan struct{})
  	ch := GernerateIntA(done)
  	fmt.Println(<-ch)
  	fmt.Println(<-ch)
  
  	close(done)
  	for v := range ch {
  		fmt.Println(v)
  	}
  }
  ```

  - 融合并发、缓冲、退出等多种机制的生成器

  ```go
  package main
  
  import (
  	"fmt"
  	"math/rand"
  )
  
  func GenerateIntA(done chan struct{}) chan int {
  	ch := make(chan int, 5)
  	go func() {
  	Lable:
  		for {
  			select {
  			case ch <- rand.Int():
  			case <-done:
  				break Lable
  			}
  		}
  		close(ch)
  	}()
  	return ch
  }
  
  func GenerateIntB(done chan struct{}) chan int {
  	ch := make(chan int, 10)
  	go func() {
  	Lable:
  		for {
  			select {
  			case ch <- rand.Int():
  			case <-done:
  				break Lable
  			}
  		}
  		close(ch)
  	}()
  	return ch
  }
  
  func GenerateInt(done chan struct{}) chan int {
  	ch := make(chan int)
  	send := make(chan struct{})
  	go func() {
  	Lable:
  		for {
  			select {
  			case ch <- <-GenerateIntA(send):
  			case ch <- <-GenerateIntB(send):
  			case <-done:
  				send <- struct{}{}
  				send <- struct{}{}
  				break Lable
  			}
  		}
  		close(ch)
  	}()
  	return ch
  }
  
  func main() {
  	done := make(chan struct{})
  	ch := GenerateInt(done)
  	for i := 0; i < 10; i++ {
  		fmt.Println(<-ch)
  	}
  	//done <- struct{}{}
  	close(done)
  	fmt.Println("stop")
  }
  
  ```

- 管道：假设一个函数的输入参数和输出参数都是相同的chan类型，则函数可以自己调用自己。**(链式调用)**

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func chain(in chan int) chan int {
  	out := make(chan int)
  	go func() {
  		for v := range in {
  			out <- 1 + v
  		}
  		close(out)
  	}()
  	return out
  }
  
  func main() {
  	in := make(chan int)
  	go func() {
  		for i := 0; i < 10; i++ {
  			in <- i
  		}
  		close(in)
  	}()
  
  	out := chain(chain(chain(in)))
  	for v := range out {
  		fmt.Println(v)
  	}
  }
  
  ```

- 有一个请求就启动一个协程进行处理

​		求和多协程处理：

```go
package main

import (
	"fmt"
	"sync"
)

// 工作任务
type task struct {
	begin  int
	end    int
	result chan<- int
}

//任务执行，计算begin到end的和

func (t *task) do() {
	sum := 0
	for i := t.begin; i <= t.end; i++ {
		sum += i
	}
	t.result <- sum
}
func main() {
	taskchan := make(chan task, 10)
	resultchan := make(chan int, 10)

	wait := &sync.WaitGroup{}

	go InitTask(taskchan, resultchan, 100)
	go DistributeTask(taskchan, wait, resultchan)
	sum := ProcessResult(resultchan)

	fmt.Println("sum =", sum)
}

func InitTask(taschan chan<- task, r chan int, p int) {
	qu := p / 10
	mod := p % 10
	high := qu * 10
	for i := 0; i < qu; i++ {
		b := 10*i + 1
		e := 10 * (i + 1)
		t := task{
			begin:  b,
			end:    e,
			result: r,
		}
		taschan <- t
	}
	if mod != 0 {
		t := task{
			begin:  high + 1,
			end:    p,
			result: r,
		}
		taschan <- t
	}
	close(taschan)
}

//读取taskchan，每个task启动一个协程处理
//等待每个task运行完，关闭结果通道

func DistributeTask(taskchan <-chan task, wait *sync.WaitGroup, result chan int) {
	for v := range taskchan {
		wait.Add(1)
		go ProcessTask(v, wait)
	}
	wait.Wait()
	close(result)
}
func ProcessTask(t task, wait *sync.WaitGroup) {
	t.do()
	wait.Done()
}
func ProcessResult(resultchan chan int) int {
	sum := 0
	for v := range resultchan {
		sum += v
	}
	return sum

}
```

- 固定工作池：通过线程池来提升服务的并发处理能力。在Go编程中，可以轻松构建固定数目的协程作为工作线程池。
  - 除了主要的`main goroutine`，还开启了如下几类`goroutine`:
    - (1) 初始化任务的协程
    - (2) 分发任务的`goroutine`
    - (3) 等待所有worker结束通知，然后关闭结果通道获取最终的结果。
  - 程序采用三个通道，分别是：
    - 传递task任务的通道；
    - 传递task结果的通道；
    - 接收worker处理完任务后所发送通知的通知。

```go
package main

import "fmt"

//工作池的协程数目

const (
	NUMBER = 10
)

type task struct {
	begin  int
	end    int
	result chan<- int
}

func (t *task) do() {
	sum := 0
	for i := t.begin; i <= t.end; i++ {
		sum += i
	}
	t.result <- sum
}

func main() {
	workers := NUMBER
	taskchan := make(chan task, 10)
	resultchan := make(chan int, 10)

	//worker信号通道
	done := make(chan struct{}, 10)
	go InitTask(taskchan, resultchan, 100)

	//分发任务到NUMBER个协程池
	DistributeTask(taskchan, workers, done)

	go CloseResult(done, resultchan, workers)

	sum := ProcessResult(resultchan)
	fmt.Println("sum = ", sum)

}

func InitTask(taschan chan<- task, r chan int, p int) {
	qu := p / 10
	mod := p % 10
	high := qu * 10
	for i := 0; i < qu; i++ {
		b := 10*i + 1
		e := 10 * (i + 1)
		t := task{
			begin:  b,
			end:    e,
			result: r,
		}
		taschan <- t
	}
	if mod != 0 {
		t := task{
			begin:  high + 1,
			end:    p,
			result: r,
		}
		taschan <- t
	}
	close(taschan)
}

//读取taskchan，每个task启动一个协程处理
//等待每个task运行完，关闭结果通道

func DistributeTask(taskchan <-chan task, workers int, done chan struct{}) {
	for i := 0; i < workers; i++ {
		go ProcessTask(taskchan, done)
	}
}
func ProcessTask(taskchan <-chan task, done chan struct{}) {
	for t := range taskchan {
		t.do()
	}
	done <- struct{}{}
}
func CloseResult(done chan struct{}, resultchan chan int, workers int) {
	for i := 0; i < workers; i++ {
		<-done
	}
	close(done)
	close(resultchan)
}

func ProcessResult(resultchan chan int) int {
	sum := 0
	for v := range resultchan {
		sum += v
	}
	return sum
}
```

- `future`模式

  - 在一个流程中需要调用多个子调用的情况，且子调用之间没有依赖，串行调用会浪费较多时间，可采用future模式。

  - future模式的基本工作原理。

    - 【1】使用`chan`作为函数参数
    - 【2】启动`goroutine`调用参数
    - 【3】通过`chan`传入参数
    - 【4】做其他可以并行处理的事情
    - 【5】通过`chan`异步获取结果

    ```go
    package main
    
    import (
    	"fmt"
    	"time"
    )
    
    type query struct {
    	sql    chan string
    	result chan string
    }
    
    func exeQuery(q query) {
    	go func() {
    		sql := <-q.sql
    		q.result <- "result from" + sql
    	}()
    }
    
    func main() {
    	q := query{
    		sql:    make(chan string, 1),
    		result: make(chan string, 1),
    	}
    	go exeQuery(q)
    	//发送参数
    	q.sql <- "select * from table"
    	time.Sleep(1 * time.Second)
    	fmt.Println(<-q.result)
    
    }
    ```

    - **future最大的好处就是将函数的同步调用转换为异步调用，适用于一个交易需要多个子调用且子调用之间没有依赖的场景。**

#### context标准库

- go中协程没有父子关系，即没有所谓子进程退出后的通知机制。
  - 通信：`chan`通道是协程之间通信的基础，这里的通信主要是指程序的数据通道。
  - 同步：不带缓冲的`chan`提供了一个天然的**同步等待机制**；当然`sync.WaitGroup`也可以为多协程提供同步机制。
  - 通知：在输入端绑定两个`chan`，一个用于业务数据流，一个用于异常通知数据，然后通过`select`进行处理。
  - 退出：通知协程退出的方式，可以借助通道和`select`的广播机制(close channel to broadcast)进行退出。**`context`库提供两种功能：退出通知和元数据传递。在内部维护一个调用树，并且在调用树中传递通知和元数据**
- 设计的目的：跟踪协程调用树，并且在这些协程调用树中传递通知和元数据
  - 【1】退出通知机制——通知可以传递给整个协程调用树上的每一个协程；
  - 【2】传递数据——数据可以传递给整个协程调用树上的每一个协程。
- 基本数据结构：**树结构，第一个创建Context的协程被称为root节点。root节点负责创建实现Context接口的具体对象，再传递给下游的协程。**
  - Context是一个基本接口，所有的Context对象都要实现该接口，context的使用者在调用接口中都适用Context作为参数类型。
  - `context.Context` 在 Go 语言的并发编程中非常重要，主要用于控制协程（goroutines）的生命周期，特别是在处理取消信号、超时以及传递请求范围的值时。

  ```go
  type Context interface{
  	//Context 实现了超时控制
  	Deadline() (deadline time.Time, ok bool)
  
  
  	Done() <- chan struct{}
  	//Done返回的chan收到通知时，才可以访问Err()货值为什么原因被取消
  	Err() error
  	//可以访问上游协程传递给下游协程的值
  	Value(key interface{}) interface{}
  }
  /*
  Deadline() (deadline time.Time, ok bool):
  这个方法返回 context.Context 的截止时间，即该上下文的超时时间点。如果设置了截止时间，ok 将为 true，否则为 false。这允许调用者知道是否有一个超时需要考虑。
  
  Done() <-chan struct{}:
  Done 返回一个类型为 chan struct{} 的通道，这个通道会在上下文被取消或超时时关闭。如果 context.Context 被取消或超时，侦听这个通道的协程可以接收到信号，从而及时停止执行或进行清理操作。
  
  Err() error:
  当 context.Context 被取消或超时时，可以通过 Err() 方法获取相应的错误信息。如果上下文还没有被取消，Err() 将返回 nil。
  
  Value(key interface{}) interface{}:
  Value 方法允许从 context.Context 中检索键对应的值。这常用于在协程之间传递请求范围的数据，例如跟踪ID或用户认证信息。
  */
  ```

- `context`用法

  ```go
  package main
  
  import (
  	"context"
  	"fmt"
  	"time"
  )
  
  type otherContext struct {
  	context.Context
  }
  
  func main() {
  	//首先通过 context.WithCancel(context.Background()) 创建一个带取消功能的 context (ctxa) 和对应的取消函数 (cancel)。
  	ctxa, cancel := context.WithCancel(context.Background())
  	go work(ctxa, "work1")
  	tm := time.Now().Add(3 * time.Second)
  	//代码通过 context.WithDeadline(ctxa, tm) 基于 ctxa 创建了一个带截止时间的 context (ctxb)。tm 是当前时间后 3 秒，意味着 ctxb 将在 3 秒后过期。
  	ctxb, _ := context.WithDeadline(ctxa, tm)
  
  	go work(ctxb, "work2")
  	oc := otherContext{ctxb}
  	//代码创建了一个 otherContext 结构体，它内嵌了 ctxb。接着通过 context.WithValue(oc, "key", "andes, pass from main") 创建了一个带值的 context (ctxc)。
  	ctxc := context.WithValue(oc, "key", "andes, pass from main")
  
  	go workWithValue(ctxc, "work3")
  
  	time.Sleep(10 * time.Second)
  	fmt.Println("begin to end")
  	cancel()
  	time.Sleep(5 * time.Second)
  	fmt.Println("main stop")
  }
  
  func work(ctx context.Context, name string) {
  	for {
  		select {
  		case <-ctx.Done():
  			fmt.Printf("%s get msg to cancel\n", name)
  			return
  		default:
  			fmt.Printf("%s is running \n", name)
  			time.Sleep(1 * time.Second)
  		}
  	}
  }
  
  func workWithValue(ctx context.Context, name string) {
  	for {
  		select {
  		case <-ctx.Done():
  			fmt.Printf("%s get msg to cancel\n", name)
  			return
  		default:
  			value := ctx.Value("key").(string)
  			fmt.Printf("%s is running value = %s \n", name, value)
  			time.Sleep(1 * time.Second)
  		}
  	}
  }
  
  ```

  - 在使用Context的过程中，程序在底层实际维护了两条关系链

    - 1、children key构成从根到叶子Context实例的引用关系，这个关系在调用With函数时进行维护。

      ```go
      ctxa.children--->ctxb
      ctxb.children--->ctxc
      ```

      - Context包的取消广播通知就是基于该树实现

    - 在构造Context的对象中不断地包裹Context实例形成一个引用关系链，这个关系链的方向时相反的，是自底向上的。**用于切断和上层Context的联系。**

- context传递数据

  - 应该传递什么数据
    - (1) 日志信息；
    - (2) 调试信息；
    - (3)不影响业务主逻辑的可选数据。

## 六、反射


### 基本概念
​	**反射是用程序检查其所拥有的结构，尤其是类型的一种能力；这是元编程的一种形式。反射可以在运行时检查类型和变量，例如：它的大小、它的方法以及它能“动态地”调用这些方法。**

- `reflect.Typeof()`用法

```go
package main

import (
	"fmt"
	"reflect"
)

type Student struct {
	Name string "student_name"
	Age  int    `a:"student_age"`
}

func main() {
	s := Student{}
	//获取类型信息
	rt := reflect.TypeOf(s)

	//尝试通过字段名"Name"获取Student的字段信息。
	fieldName, ok := rt.FieldByName("Name")
	if ok {
		fmt.Println(fieldName.Tag)
	}

	fieldAge, ok2 := rt.FieldByName("Age")
	if ok2 {
		fmt.Println(fieldAge.Tag.Get("a"))
		//fmt.Println(fieldAge.Tag.Get("b"))
	}
	//打印Student类型的各种信息，包括类型名、字段数、包路径、类型的字符串表示，以及所有字段的名字。
	fmt.Println("type_Name:", rt.Name())
	fmt.Println("type_NumField:", rt.NumField())
	fmt.Println("type_pkgPath:", rt.PkgPath())
	fmt.Println("type_String:", rt.String())

	for i := 0; i < rt.NumField(); i++ {
		fmt.Printf("type_Field[%d].Name:=%v\n", i, rt.Field(i).Name)
	}
	sc := make([]int, 20)
	sc = append(sc, 1, 2, 3)
	//通过反射获取了切片元素的类型。然后打印出元素类型的种类
	sct := reflect.TypeOf(sc)
	//得到的是切片中元素的类型信息。
	scet := sct.Elem()
	//打印了切片元素类型的字符串表示、名称、方法数量、包路径等信息。
	fmt.Println("slice element type.Kind()=", scet.Kind())
	fmt.Printf("slice element type.Kind()= %d\n", scet.Kind())
	fmt.Println("slice element type.String()=", scet.String())

	fmt.Println("slice element type.Name()=", scet.Name())
	fmt.Println("slice type.NumMethod()=", scet.NumMethod())
	fmt.Println("slice type.PkgPath()=", scet.PkgPath())
	fmt.Println("slice type.PkgPath()=", sct.PkgPath())

	fmt.Println()

}
//student_name
//student_age
//type_Name: Student
//type_NumField: 2
//type_pkgPath: main
//type_String: main.Student
//type_Field[0].Name:=Name
//type_Field[1].Name:=Age
//slice element type.Kind()= int
//slice element type.Kind()= 2
//slice element type.String()= int
//slice element type.Name()= int
//slice type.NumMethod()= 0
//slice type.PkgPath()=
//slice type.PkgPath()=
```

- **对于`reflect.TypeOf()`**如果传进去的是一个接口变量，如果接口变量没有绑定具体类型实例，则返回的是接口自身的静态类型信息，；如果绑定具体类型实例，则返回的是具体类型实例的信息。
- `refelct.Value`，表示实例的值的信息

```go
package main

import (
	"fmt"
	"reflect"
)

type User struct {
	Id   int
	Name string
	Age  int
}

func (this User) String() {
	println("User:", this.Id, this.Name, this.Age)
}
func Info(o interface{}) {
	//首先获取对象o的值和类型
	v := reflect.ValueOf(o) //包含了关于 o 的信息和可以对 o 进行操作的方法。
	t := v.Type()
	//打印类型名
	println("Fields")
	//打印字段信息:
	for i := 0; i < t.NumField(); i++ {
		//获取字段的元数据（比如字段名和类型）
		field := t.Field(i)
		//获取字段的实际值
		value := v.Field(i).Interface()
		switch value := value.(type) {
		case int:
			//%6s 表示字段名以最小宽度6字符右对齐打印，%v 表示以默认格式打印字段类型，而 %d 或 %s 则根据值的类型打印整数或字符串。
			fmt.Printf("%6s: %v = %d\n", field.Name, field.Type, value)
		case string:
			fmt.Printf("%6s: %v = %s\n", field.Name, field.Type, value)
		default:
			fmt.Printf("%6s: %v = %s\n", field.Name, field.Type, value)

		}
	}
}

func main() {
	u := User{
		Id:   1,
		Name: "Tom",
		Age:  30,
	}
	Info(u)
}
//Type:  User
//Fields
//Id: int = 1
//Name: string = Tom
//Age: int = 30

```

- 底层类型和基础类型

  - 基础类型是抽象的类型划分，底层类型是针对每一个具体的类型来定义的，比如不同的struct在基础类型上都划归为struct类型，但不同的struct底层类型是不同的。

- 类型汇总
	
		简单类型 复合类型 类型字面量 自定义类型
		命名类型 未命名类型
		接口类型 具体类型
		底层类型 基础类型
		动态类型 静态类型

### 反射规则
