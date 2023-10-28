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
- `
>>>>>>> 4248ee7 (GolangNote)
