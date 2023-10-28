# Go

## 一、基础概念
#### 1、Go语言的核心特性：类型系统、接口、并发。
- Go语言为了解决当下编程语言对并发支持不友好，编译速度慢、编程复杂这三个问题而诞生的。
- 特征解读：
	- 函数以`func`为开头，函数体开头的`{`必须在函数头所在行尾部，不能单独起一行。
	- `main`函数所在报名必须是`main`

#### 2、类型

<<<<<<< HEAD
=======



#### 3、if语句
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
#### 4、switch语句

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

#### 5、for循环

- 1、类似 C 里面的for循环语句
		for init; condition; post{}
- 2、类似 C 里面的 while 循环语句
		for condition{}
- 3、类似 C 里面的 while(1) 死循环语句
		for {}
- 4、对数组、切片、字符串、map和通道的访问，语法格式如下
	
		//访问map
		for key, balue := range map{}
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
		for value := krange channel{}

#### 6、标签和跳转
- 标签：使用标签来标识一个语句的位置，用于goto 、break、continue语句的跳转。
	
		Lable: Statement
	
- goto语句有以下几个特点：
	- goto语句只能在函数内跳转
	- goto语句不能跳过内部变量的声明语句，例如
			goto L //BAD，跳过v := 3 不被允许
			v := 3
			L:
	- goto语句只能挑到同级作用域或者上层作用域内，不能跳到内部作用域内，例如：
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

## 二、函数
#### 实参到形参的传递
- go函数实参到形参的传递永远都是值拷贝，有时函数调用后实参指向的值发生了变化，那是因为参数传递的是指针值的拷贝，实参是一个指针变量，传递给形参的是这个指针变量的副本，二者指向同一地址，本质上参数传递仍然是值拷贝。

- 不定参数：不定参数声明使用`param ...type`的语法格式
<<<<<<< HEAD
- `
>>>>>>> 4248ee7 (GolangNote)
=======

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
- recover()用来捕获panic，阻止panic继续向上传递。recover()和defer一起使用，但是只有在defer后面的函数体内被直接调用才能捕获panic终止异常，否则返回nail

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
deder func(){
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

#### 错误处理

- 

- 错误和异常

  - 狭义的错误：发生非期望的已知行为，这里的已知是指错误的类型是预料并定义好的。

  - 异常：发生非期待的未知行为。异常又被称为未捕获的错误。比如C中的段错误。

  - 错误可以分为两类：

    - 运行时错误，可以被捕获，并采取措施——隐式抛出panic

    - 程序逻辑错误

## 三、类型系统

#### 命名类型和未命名类型

- 命名类型：命名类型可以通过标识符来表示，包括自定义类型。

- 未命名类型：一个类型由预声明类型、关键字和操作符组合。复合类型：数组、切片、字典、通道、指针、函数字面量、结构和接口。其中`*int`, `[]int`, `[2]int`, `map[k]v`

  - 类型字面量：类型字面量不是使用预先定义的类型名称，而是直接在声明或者使用的地方定义了类型的结构。这通常用于创建匿名结构体、匿名接口、切片类型、映射（map）类型、通道（channel）类型等。

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

  - 类型赋值

    - 不同类型之间的变量之间一般不能直接相互赋值

    - a可以赋值给变量b必须要满足如下条件中的一个

      - 1、T1 和 T2的类型相同

      - 2、T1 和 T2 具有相同的底层类型，并且T1 和 T2中至少有一个是未命名类型

      - 3、T2是接口类型，T1是具体类型，T1的方法集是T2方法集的超集

      - 4、T1 和 T2 都是通道类型，拥有相同的元素类型，并且T1 和 T2中至少有一个是未命名类型。

      - 5、a是预声明标识符nail， T2是pointer、function、slice、map、channel、interface等类型

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

      

>>>>>>> ba22fab (update the goNote by the MAC)
