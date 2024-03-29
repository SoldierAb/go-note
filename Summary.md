# golang note

## 基础语法

### 语言特性

    + 自动垃圾回收
    + 并发编程
    + 多返回值

### 变量

    

``` go
        //定义
        var v1 int
        var ar [10] int   //数组
        var arr [] int    //数组切片
        var po *int       //指针
        var m map[string] int   //map key为string类型 value为类型
        var f func(a int) int 

        //初始化 
        var a int = 10;
        var a = 10;
        a := 10     //自动推导

        var b int;
        b := 10
        /*-------------------
        no new variables on left on side of := 
        := 左侧的变量必须为未声明过的
        */

        //赋值
        var a int;
            a=10;

        // 交换
        i,j = j,i

        //匿名变量
        func GetName()(firstName,lastName string){
            return "kobe","bryant"
        }

        _,lastName := GetName();   //只获取 lastName 
 
```

### 常量

* 字面常量

    ```go
            -12
            3.1415994329423424   //浮点类型
            3.2+12i              //复数类型
            true                 //布尔类型
            "foo"                //字符串类型
    ```

* 常量定义
    + 通过 const 关键字

        ```go
            const Pi  float64 = 3.1435235235345235254255523
            const zero = 0.0        //无类型浮点常量
            const (
                size int64 = 1024
                eof = -1
            )                       //无类型整型常量

            const a,b float32 = 0,1    //a=0.0 b=1.0  常量的多重赋值

            const a,b,c = 3,4,"foo"    //a=3 b=4 c="foo" 无类型整型和字符串常量

        ```

    + 预定义常量

        true,false,iota
        iota 可认为是一个可被编辑器修改的常量，在每一个const出现时被重置为0,在下一个const出现之前，每出现一次iota，其代表的数字就会自增1
        ```go
            const (
                a=iota  //a==0
                b=iota  //b==1          
                c=iota  //c==2
            )
            const (
                a=1<<iota   //a==1
                b=1<<iota   //b==2         
                c=1<<iota   //c==4
            )

            const (
                a = iota * 12          //0 
                b float64 = iota * 12  //12.0
                c = iota * 12          //24
            )
            const x iota //0
            const y iota //0
            
            //如果const 的赋值表达式是一样的，可以只保留首个表达式,如前两个例子等同于：
            const (
                a=iota
                b
                c
            )

            const (
                a = 1<<iota
                b, 
                c
            )
        ```

### 枚举

* 定义
    + 通过const () 定义一组常量来定义枚举值

        ```go
            const (
                Sunday = iota
                Monday,
                Tuesday
                Wednesday
                Thursday
                Friday
                Saturday
                numberOfDays  //这个常量没有被导出，包内私有
            )

            //同其他符号（symbol）一样 ，大写字母开头的常量在包外可见

        ```

### 类型

* 基础类型
    + 布尔类型：bool
        - 可赋值为预定义的true和false

            ```go
                var v bool
                v = true
                v2 := (1==2)
                //不接受其他类型的赋值，不支持自动或强制类型的转换，如下
                var b bool
                b = 1       //编译报错
                b = bool(1)   //编译报错
            ```

    + 整型： int8, byte, int16, uint16, int32, uint32, int64, uint64, unitptr 等

        
        

![整型数据表](./assets/img/int-table.jpg)

        1.  类型表示：

            需要注意的是int和int32为不同类型，编译器不会自动转换，要转换需使用强制类型转换的方式
            ```go
                var val1 int32
                val2 := 64    //int 型
                val1 = val2    //编译出错
                val1 = int32(val2)   //编译通过
            ```
 `强制转需要注意的是精度丢失问题` 

        2. 数值运算: + - * / % 
        3. 比较运算： < ,<= , == ,>=,>,!= 

 `两个不同类型的值是不能直接比较，但是各种类型的整型可以与字面常量（literal）进行比较` 
            ```go
                    var i int32
                    var j int64
                    i, j = 1, 2
                    if i==j {        //编译出错
                        fmt. Println("----"); 
                    }

                    if i==1 || j ==2 {  //编译通过
                        fmt.Println("----");
                    } 
            ```

        4. 位运算

            

![位运算](./assets/img/wys.jpg)

    + 浮点型： 

        

        - float32（等价于 C语言的 float型）,
        - float64 （等价于 C 语言的double型），转换同整型一样需要使用强制类型转换 
        - 比较

            因为浮点数不是一种精确的表达式，不能直接像整型一样使用 == 来判断是否相等，推荐以下方案：
            ```go
                import "math"

                // p 为用户自定义的比较精度 ， 如 0.000001
                func IsEqual(f1,f2,p float64) bool{
                    return math.Fdim(f1,f3) < p 
                }

            ```

    + 复数类型：complex64, complex128

        ```go
            //定义：实部 + 虚部
            var val1 complex64     //由2个float32构成的复数类型
            val1 = 3.2 + 12i
            val2 := 3.2 + 12i     //val2为complex128 类型
            val3 := complex(3.2,12)    //同val2

        ```

    + 字符串： string

        ```go
            var str string
            str = "hello world"
            ch := str[0]    //取字符串第一个字符
            //注： 字符串的内容不能在初始化后被修改 ，如
            str[0] = "x"     //编译出错
        ```

        - 字符串操作：

            

![字符串操作](./assets/img/string.jpg)

        - 遍历：

            ```go
                str := "hello 世界"
                n := len(str)
                
                //Unicode字符方式遍历，元素值为rune类型
                for i,ch := range str{ 
                    fmt.Println(i,ch)
                }

                //字节数组方式遍历，元素值为byte类型
	            for i:=0;i<len(str);i++ {
		            fmt.Println(i,str[i]);   
	            }

            ```

    + 字符类型：
        - byte (**实际是uint8的别名**) 代表UTF-8字符串单个字节的值
        - rune 代表单个Unicode字符

    + 错误类型： error

    

* 复合类型
    + 指针 pointer
    + 数组 array

        ```go
            [32]byte     //长度为32的数组，每个元素占一个字节
            [2*N] struct {x, y int32}  //复杂类型数组
            [1000]*float64      //指针数组
            [2][3] int         //二维数组
            [2][3][4]float64      //等同于 [2]([3]([4]float64)) 
            
            arrLen := len(arr)    //数组长度len是数组内置常量 
            
        ```
        数组访问:
        ```go
            for i:=0; i<len(arr) ; i++{
                fmt. Println(i, arr[i])
            }

            for i, v := range arr {
                fmt. Println(i, v); //下标 , 值
            }
        ```

        值类型
        ```go
            func modifyArr(arr [10]int){
                arr[0] = 11
                fmt.Println('modify arr',arr)
            }

            func main(){
                arr := [5]{1, 2, 3, 4, 5} 
                modify(arr)        //[11, 2, 3, 4, 5]
                fmt. Println('main ', arr); //[1, 2, 3, 4, 5]
            }
            //在go中，数组是一种值类型，所有的值类型变量在赋值和作为参数传递时将产生一次复制动作。如果将数组作为函数的参数类型，则在函数调用时将发生数据复制，因此在函数体中无法修改传入的数组内容, 函数体操作的是该参数的一个副本。
        ```
$mdFormatter$36$mdFormatter$

    + 切片 slice
        - 数组切片
            - 基于数组

                ```go
                 func main(){
                     var arr [5]int = [5]int{1,2,3,4,5}
                     var mySlice  []int =  arr[:3]
                     
                     for i,v := range arr{
                         fmt.Println(i,v)  //1,2,3,4,5
                     }

                     for i,v := range mySlice{
                         fmt.Println(i,v)   //1,2,3
                     }
                 }

                 // go支持以arr[start:end] 形式创建数组切片
                 arr[:]     //基于所有数组元素创建切片
                 arr[2:]    //基于第2个数组元素开始的所有元素创建切片 index 2 ~ len(arr)-1
                 arr[:5]    //基于前5个元素创建切片 index 0~4
                
                 // index ~ [start,end)
                ```

            - 直接创建 **make**

                ```go
                    //创建一个初始元素个数为5的切片，元素初始值为0
                    mySlice := make([]int,5)
                    
                    //创建一个初始元素个数为5的切片，元素初始值为0,并预留10个元素的存储空间
                    mySlice :=make([]int,5,10)

                    //直接创建并初始化包含3个元素的数组切片
                    mySlice :=[]int{1,2,3}

                ```

            - 动态增减元素

                数组切片支持内置的**cap**和**len**函数，
                **cap**返回的是数组切片分配的空间大小
                ```go
                    func main(){
                        mySlice :=  make([]int, 5, 20)
                        fmt. Println(len(mySlice), cap(mySlice))  //5, 20
                    }
                ```

                新增元素： **append**
                ```go
                    //尾部添加新元素
                    mySlice = append(mySlice,1,2,3)

                    //数组切片追加
                    aSlice  := []int{7, 8, 9}
                    mySlice  = append(mySlice, aSlice...) //省略号，打散后传入，等同于： mySlice = append(mySlice, 7, 8, 9)
                    
                ```

                - `注：` 数组切片会自动处理存储空间不足的问题，如果追加的内容超过当前已经分配的存储空间大小（**cap**返回的信息），数组切片会自动分配一块足够大的内存

            

            - 基于数组切片创建数组切片

                ```go 
                    oldSlice := []int{1, 2, 3}
                    newSlice := oldSlice[:2]
                ``` 
 `注:` **newSlice选择oldSlice元素的范围可超过其所包含的元素个数，只要范围不超过oldSlice的存储能力范围（cap）, newSlice超过oldSlice元素的部分会自动填上0**
            

            - 内容复制

                **copy(paramA,paramB)** paramA 按其中较小的切片的元素个数进行复制替换
                ```go
                    sliceA := []int{1,2,3}
                    sliceB := []int{4,5,6,7}

                    copy(sliceB,sliceA);  
                    //sliceB[1,2,3,7]  sliceA[1,2,3]

                    //copy(sliceA, sliceB)
                    //sliceA[4, 5, 6]     sliceB[4, 5, 6, 7]
                ```

    + 字典 map

        ```go
            type PersonInfo struct {
                ID string
                Name string
                Address string
            }

            func main(){
                var personDB map[string] PersonInfo
                personDB = make(map[string] PersonInfo)

                //数据插入
                personDB["123456"] = PersonInfo{"123456","Kobe","room 12 ..."}
                personDB["2"] = PersonInfo{"2","Jordon","room 34"}

                //数据查找
                person,ok := personDB["123456"];
                //ok 返回的bool类型，如果找到对应数据则为true
                if ok {
                    fmt.Println("found person by ID = 123456");
                }else{
                    fmt.Println("did not found");
                }

                //删除
                delete(PersonDB,"2")
                person2,founded := PersonDB["2"];
                fmt.Println(person2,founded);   //{0  } false
            }

        ```

    + 通道 chan

    + 结构体 struct

    + 接口 interface

### 流程控制

* 条件语句 if、else 、else-if

    
 ` * 注： 在有返回值的函数中，不允许将“最终”的return语句包含在if...else...结构中` 

* 选择语句 switch case 、select

* 循环语句 for 、range

  ```go
    //简化无限循环
    
    sum := 0
    for{
        sum++
        if sum>100{
            break
        }
    }

    //条件表达式支持多重赋值

     a := []int{1,2,3,4,5}
     for i,j := 0,len(a); i<j; i,j := i+1,j-1{
         a[i],a[j] = a[j],a[i]
     }

  ```
 `*注： 循环同样支持continue和break来控制循环：` 
    ```go
    for i :=0 ; i< 5 ;i++{
        for j:=0;j<10;j++{
            if(j>5){
                break JLoop
            }
            fmt.Println(i,j);
        }
    }
    JLoop:
    //...
    //本例中，break终止的是JLoop标签处的外层循环
    ```

* 跳转语句 goto

### 函数

* 定义

    ```go
        func myFunc(a,b int)(res int,err error){
            if a<0 || b<0{
                err = error.New("——————————")
            }
            return a+b,nil    //支持多重返回值
        }
    ```
 `注： 小写字母开头的函数只在本包内可见，大写字母开头的函数才能被其他包使用，这个规则也适用于类型和变量的可见性` 

* 调用

    导入函数所在的包就可以直接调用

* 不定参数类型（不定参数数量）

    ```go
        func myFunc(args ...int){
            for i,v := range args{
                fmt.Println(i,v)
            }
        }

        //调用
        myFunc(1,2,3,4)
        myFunc(1,2)

    ```

* 不定参数的传递

```go
    func myFunc2(args ...int){

    }
    func myFunc(args ...int){
        //原样传递
        myFunc2(args...)
        //传递片段
        myFunc2(args[2:]...)
    }
```    

* 任意类型的不定参数

    
    希望传递任意类型，可指定类型为interface{}

    ```go
        func myFunc(args ...interface{}){
            for i, arg := range args{
                fmt. Println(i, "arg type", arg.(type)); 
            }
        }
    ```
    

* 多返回值

        

* 匿名函数与闭包

    ```go
        i:=11
        fn:=func ()(func ()){
            var a int =10
            return func(){
                fmt.Println(i,a)
            } 
        }

    ```

### 错误处理

---

## OOP

###  为类型添加新方法

	```go 
		type Integer int

        func (a Integer) Less(b Integer) bool{
	        return a<b
        }

        func (a *Integer) Add(b Integer){
	        *a += b
        }

        func main(){
	        var a Integer = 2
	        if a.Less(5){
		        fmt.Println(a,"less 5")
	        }

	        a.Add(9)
	        fmt.Println("a add 9 equal : ",a)

	        var arr = [3]int{1,2,3}
	        var b = arr

	        b[1]++

	        fmt.Println(arr,b)

	        var c=&arr
	        c[1]++
			fmt.Println(arr,*c)
		}
	```
### 初始化、创建函数（“构造函数”）

	```go
		//初始化
	type Rect struct{
		x,y float64
		width,height float64
	}

	rect1 := new(Rect)
	rect2 := &Rect{0,0,2,4}
	rect3 := &Rect{width:200,height:400}
	rect4 := &Rect{}

	//全局创建函数
	func NewRect(x,y,width,height float64) *Rect{
		return &Rect{x,y,width,height}
	}

	```

### 匿名组合
	```go
		type Base struct {
			Name string
		}

		func(b *Base) foo(){
			...
		}

		func(c *Base) bar(){
			...
		}
	```


### 可见性

![可见性](./assets/img/oop-see.png)

### 接口

- 非侵入式接口
    
    一个类只要实现了这个类的所有接口，就可以说这个类实现了这个接口
    ```go
        type IReader interface {

        }
    ```

- 接口赋值

    接口B的方法列表是A的方法列表的子集，则A可以赋值给B，反之不行

- 接口查询

    ```go
    
    ```

- 接口组合



- Any 类型


## 08.07 - 并发编程
- 协程（goroutine） `go`关键字
- 并发通信：
    - 锁（繁琐、臃肿）

- channal 
    - 基本语法：
        - 声明 `chan`关键字添加
        - 定义 make  ,  缓冲机制
    
    - select
        - 处理异步IO
        - case 语句必须是IO操作（读取、写入）
    
    - 超时机制
        - 利用select实现

    - channel 传递
    - 单向channel  , 转换
    - 关闭
        - close
        ```go
            x,ok := <- ch   //于map按键获取类似，ok为bool
        ```    


##  0808

- IPC简单示例 demo编写学习【server端和client端】
    
    包含知识点：
    - 用channel作为模块之间的通信方式
  

- defer
    - go的延迟函数
    - 函数不会立即被调用
    ```go

		import (
			"fmt"
			"os"
		)

		func ReadFile(fileName string){
			f,err := os.Open(fileName)

			if err !=nil{
				fmt.Println("read file failed:   ",fileName)
				return
			}

			defer f.Close()

			var content [1024]byte
			f.Read(content[:])
			fmt.Println("%s",content)
		}

		func main(){
			ReadFile("test.json")
		}

    ```
				
    - 函数值和函数参数被求值，但函数不会立即调用:
		```go

		import (
			"fmt"
			"log"
			"time"
		)

		func Trace(funcName string) func(){
			start := time.Now()
			fmt.Printf("func s% enter \n",funcName)
			return func() {
				log.Printf("func s% exit s% ",funcName,time.Since(start))
			}
		}


		func foo(){
			defer Trace("foo()")()
			time.Sleep(2*time.Second)
		}

		func main(){
			foo()
			foo()
		}

		```
	- 杂项
		- 如果存在多个defer ,则defer的执行顺序与出现顺序相反
 
## go sync 包
- Mutex (互斥锁)
	- Lock() 加锁 Unlock()解锁
	- 在一个goroutine获的Mutex之后，其他goroutine只能等待这个goroutine 释放这个Mutex

	- 使用Lock()之后，不能再继续对其加锁，直到UnlocK()之后才能继续加锁

	- 在Lock之前使用Unlock会导致panic异常

	- 已经锁定的Mutex并不与特定的goroutine相关联，即可以用一个goroutine对其加锁，用另外一个goroutine对其解锁

	- 在同一个goroutine 中的Mutex 解锁之前加锁，会导致死锁

	- 适用与读写不确定，并且只有一个读或者写的场景

	
- RWMutex (读写锁 - 基于 Mutex实现)
		
	- 单写多读锁，该锁可以加多个读锁单个写锁
		
	- 读锁占用的情况下不会阻止写和读，多个goroutine 可以同时获取读锁
		
	- 写锁会阻止其他goroutine进入（不论是读还是写），整个锁由该goroutine独占
	- 适用于读多写少的场景
	

- Lock 、 Unlock(加写锁、解写锁)

	- 如果在加写锁之前已经有其他的写锁或者读锁，则Lock()会阻塞直到该锁可用，为确保该锁可用，已经阻塞的Lock()调用会从获得的锁中排除新的读取器，即 **写锁权限高于读锁，有写锁时先进行写锁定**

	- 在Lock之前使用Unlock会导致panic异常
	
	- RLock 、RUnlock(加读锁、解读锁)
	- RLock加读锁时，如果存在写锁则无法加读锁；当只有读锁或者没有锁时，可以加多个读锁
	- RUnlock 解读锁，RUnlock撤销RLock调用，对于其他同时存在的读锁则没有效果 
	- 在没有读锁、RUnlock的个数多于RLock的个数的情况下调用RUnlock会导致panic错误

	
		


		
    

