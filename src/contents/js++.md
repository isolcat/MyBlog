---
title: js++
datetime: 2023-1-12T19:38:11Z
featured: false
draft: false
tags:
  - 面试题
description: js进阶内容
---

# js++

### 劝君专注案前事，亦是杯酒敬前程

### 预编译

1. 检查通篇的语法错误
2. 解释一行，执行一行

- 函数声明和变量声明
  - 函数声明整体提升，变量只有声明提升，赋值不提升



```js
test() // 1
function test(){
    console.log(1)
}

console.log(a) // undefined
var a = 10;

```



### 暗示全局变量

- 即任何变量如果变量未经声明赋值，此变量就为全局对象(window)所有

```js
function test(){
	a = 10;
    var c = b = 1;
}
test();
console.log(a);// 10
console.log(b);// 1
console.log(c);//c is not defined
console.log(window.c); // undefined
```

- 全局上的任何变量都归全局对象(window)所有,window 就是全局的域

```js
var a = 10;
console.log(window.a);//10
```



### 函数上下文

- **AO**

  AO步骤：

1. 创建AO（Activation Object）对象，又叫执行期上下文；
2. 寻找形式参数和变量声明作为AO的属性名，并赋值为undefined；
3. 传入实际参数的值；
4. 在函数体内寻找函数声明，放入作为AO的属性，并赋值为其函数体。

```js
function test(a){
    console.log(a)
    var a = 1;
    console.log(a)
    function a (){}
    console.log(a)
    var b = function(){}
    console.log(b)
    function d(){}
}
test(2)
/*
	function a (){}
	1
	1
	function(){}
*/
```



```js
function test(a,b){
 	console.log(a);
    c = 0;
    var c;
    a = 5;
    b = 6;
    console.log(b);
    function b(){};
    function d(){};
    console.log(b);
}
test(1);
AO {
    a:undefined -> 1 ->(执行函数体) -> 5
    b:undefined ->(执行函数体) ->  function b(){} -> 6
    d : undefined -> (执行函数体) -> function d(){} 
    c: undefined -> (执行函数体) -> 0
}

// 输出
1
6
6

```



### 全局上下文

- GO global object 全局上下文
  1. 找变量
  2. 找函数声明
  3. 执行

```js
var a = 1;
function a(){
    console.log(2)
}
console.log(a) // 1

GO = {
	a:undefined -> function a(){} -> 1
}
```



```js
console.log(a,b) // function a(){},undefined
function a(){}
var b = function(){}
```



- 综合练习

  ```js
  var b = 3;
  console.log(a)；
  function a(a){
      console.log(a);
      var a = 2;
      console.log(a);
      function a(){
          var b = 5;
          console.log(b)
      }
  }
  a(1)
  // 输出
  function a(){}
  1
  2
  5
  ```

​	

```js
a = 1;
function test(){
    console.log(a);
    a = 2;
    console.log(a);
    var a = 3;
    console.log(a);
}
test();
var a;
// 输出 
undefined
2
3
```



```js
function test(){
    console.log(b);
    if(a){
        var b = 2;
    }
    c = 3;
    console.log(c);
}
var a;
test();
a = 1;
console.log(a);

// 输出
undefined
3
1
```



```js
function test(){
    return a;
    a = 1;
    function a(){}
    var a = 2;
}
console.log(test());// ƒ a(){}

function test(){
    a = 1;
    function a(){};
    var a = 2;
    return a;
}
console.log(test()) // 2
```



```js
a = 1;
function test(e ){
    function e(){}
    arguments[0] = 2;
    console.log(e);
    if(a){
        var b = 3;
    }
    var c;
    a = 4;
    var a;
    console.log(b);
    f = 5;
    console.log(c);
    console.log(a);
}
var a;
test(1);
console.log(a);
console.log(f);

GO:{
    a:undefined
    test:function test(){...}
    f:5
}
AO:{
    e:undefined
    	1
    	function e (){}
    	2
    a:undefined
    	4
    c:undefined
    b:undefined
}
// 输出
2
undefined
undefined
4
1
5
```





### 隐式类型转换

```js
if(typeof(a) && (-true) + (+undefined) + '')
    {
        console.log('通过了')
    }else{
        console.log('没通过')
    }
// 通过了
a = undefined typeof(a) = 'undefined' // string 为正
-true = -1
+undefined = NaN
(-true) + (+undefined) + '' = 'NaN'
```







### 作用域，作用域链

- AO -> function 独立的仓库
- 函数也是一种对象类型，是一种引用类型
- 函数内部有些属性是我们无法访问的，JS引擎内部固有的隐式属性

- [[scope]]
  每个JavaScript函数都是一个对象，对象中有些属性我们可以访问， 有些不可以，这些属性仅供javaScript引擎存取,[[scope]]就是其中一个[[scope]]指的就是我们所说的作用域，其中存储了运行期上下文的集合。

- 作用域链
  [[scope]]中所存储执行期上下文对象的集合，这个集合呈链式连接，我们把这种链式连接叫作用域链

```js
function a(){
    console.log(1)
    function b(){
        console.log(2)
        function c(){
        	console.log(3)
        }
        c()
    }
    b()
}
a()
//
1
2
3
// 作用域链过程
a定义: a.[[scope]] -> 0:GO
a执行: a.[[scope]] ->	0:a -> AO
					 1:GO
b定义：b.[[scope]] -> 0:a -> AO
					 1:GO
b执行: b.[[scope]] -> 0:b -> AO
    				 1:a -> AO
					 2:GO                     
```





### 闭包

- 闭包是一种现象
- 当内部函数被返回导外部并保存时，一定会产生闭包，闭包会产生原来的作用域链不释放，过度的闭包可能会导致内存泄漏，或加载过慢
- 当函数的执行完毕后，自身的AO仍未消失,仍然被其他所引用

```js
function test1() {
    function test2(){
    var b = 2
    console.log(a+b)
}
var a = 1
return test2;
}
var test3 = test1()
test3() //3
```



```js
function test(){
    var n = 100;
    function add(){
        n ++;
        console.log(n);
    }
    function reduce(){
        n --;
        console.log(n);
    }
    return [add,reduce];
}
var arr = test()
arr[0]() // 101
arr[1]() // 100
```



```js
function sunSched(){
    var sunSched = '';
    var operation = {
        setSched:function(thing){
            sunSched = thing;
        },
          showSched:function(){
            console.log(thing)
        }
    }
        return operation;
}
```

- 闭包的另一种写法，在window上直接挂载

```js
function test(){
    var a = 1
    function plus(){
        a ++;
        console.log(a)
}
    window.plus = plus
}

```

- 还有一种写法，立即执行函数

```js
var add = (function(){
    var a = 1;
    function add(){
        a++;
        console.log(a);
    }
    return add;
})()
add()
```



### 立即执行函数

- 自动执行，执行完成之后立即释放
- IIFE - immediately -invoked function expression

```js
// 两种写法
(function(){
    
})();// 常用

(function(){
    
}()) // W3C建议
```

```js
(function(a,b){
    console.log(a,b)
}(2,4)) // 6
```

- 如何拿到返回值

```js
var num = (function(a,b){
	return a + b
}(2,4))
console.log(num) // 6
```

- 一定是表达式才能被执行符号 " () " 执行

```js
(function test1(){
    console.log(1)
})() // 1

var test2 = function(){
    console.log(1)
}() // 1

function test3(){
    console.log(1)
}() // 报错
// 转换成表达式
// 函数声明变成表达式的方法
+|-|!|& function test3(){
    console.log(1)
}() //1

function test3(){
    console.log(1)
}(2) // 这样不会报错，因为把(2)识别成表达式 
```

- 一道经典面试题

```js
function test(){
    var arr = [];
    for(var i = 0; i < 10; i ++){
        arr[i] = function(){
            console.log(i)// 打印十个10
        }
    }
    return arr;
}
var myArr = test();
for(var j = 0; j < 10; j ++){
    myArr[j]();
}
// 分析
实际上是一个闭包现象
for循环里的i对于function来说是全局变量，function里的i一直都是指向全局变量i的地址，test执行完毕后，i的地址仍被function所引用
// 怎么改成从0到9依次打印，利用立即执行函数
function test(){   
    var arr = [];
    for(var i = 0; i < 10; i ++){
        (function(j){
         arr[i] = function(){
            console.log(j)
         }            
        })(i)
    }
    return arr;
}
var myArr = test();
for(var j = 0; j < 10; j ++){
    myArr[j]();
}
```

- 一道面试题

```js
var a = 10;
if(function b(){}){ //(function b(){}) 是一个表达式 声明不了函数
    a += typeof(b)
}
console.log(a) // 10undefined
```



### 逗号运算符

- **逗号操作符** 对它的每个操作数求值（从左到右），并返回最后一个操作数的值。

```js
console.log((2,3))// 3
```

```js
var fn = (
	function test1(){
		return 1
	},
	
	function tets2(){
		return '2'
	}
)()
console.log(typeof(fn)) // string
```



### 对象

- 使用字面量创建

  ```js
  var obj = {
  	name:"张三",
  	sex:'male'
  }
  ```

- 使用系统自带的构造函数

  ```js
  var obj = new Object()
  obj.name = '张三'
  obj.sex = '男士'
  ```

- 自定义构造函数

  ```js
  // 大驼峰声明函数
  function Teacher(){
  	this.name = '张三'
      this.sex = 'male'
      this.smoke = function(){
          console.log('i am smoking')
      }
  }
  var teacher = new Teacher()
  ```

- 构造函数里的this指向实例化对象

- this原理

- 在new的时候AO里生成this对象，赋值完成后隐式的return到GO里

```js
GO = {
    Car:(function)
    car1:{
    	color:'red',
	    brand:'Benz'
	}
}
AO = {
    this:{
        color:color,
        brand:brand
    }
}
function Car(color,brand){
    this.color = color,
    this.brand = brand
    // 隐式的return this
}
var car1 = new Car('red','Benz')
```

- 利用构造函数实现累乘和累加

```js
function Compute(){
    var args = arguments
    this.loop = function(method){
        	let res
            if(method === 'plus'){
                	res = 0;
           		for(var i = 0; i < args.length; i++){
					res += args[i]
        		}
            }else if(method === 'times'){
                res = 1;
           		for(var i = 0; i < args.length; i++){
					res *= args[i]
        		}
            }
        return res
    } 
}
```



### 包装类

- 原始值没有自己的方法和属性(基础数据类型)

```js
var b = new Number(1)
console.log(b)
// Number {1}
typeof b 
//'object'
b + 1  // 2 自动拆箱

var a = 1
typeof a // 'number'
a.toString()  // '1' 自动装箱 
```

- 自动装箱过程(包装类)

```js
var b = 123
b.toString()
/*
	var c = new Number(b)
	return c.toString()
*/

```

- 一道面试题

```js
var name = 'languiji';
name += 10;
var type = typeof(name)
if(type.length === 6){
    type.text = string; // new String(type).text = 'string' delete
}
console.log(type.text) // undefined
```

- 题

```js
function Test(){
    var d = 1;
function f(){
    d++;
    console.log(d)
}
this.g = f
}
var test1 = new Test()
test1.g() // 2
test1.g() // 3
var test2 = new Test()
test2.g() // 2
```

- 面试题

```js
var x = 1,
	y = z = 0;
function add(n){
	return n = n + 1;
}
y = add(x);
function add(n){
	return n = n + 3;
}
z = add(x);
console.log(x,y,z) // 1,4,4
```



### 截断数组

```js
var arr = [1,1,23,12,421,1]
arr.length = 3
console.log(arr) // [1,1,23] 截断了数组，只剩下三个元素
```



### 原型和原型链

- 原型(prototype)，是构造函数身上的一个对象(本质上是实例化对象上的)，其包含constructor指向自身，\__proto__指向Object顶级对象的原型对象，还有一些属性和方法为实例化对象所共享 

- \__proto__ 和prototype均可更改 

```js
function Person(){}
Person.prototype.name = '张三'
var p1 = {
    name:'李四'
}
var person = new Person()
console.log(person.name) // 张三
person.__proto__ = p1
console.log(person.name) // 李四
```

- 如何理解 prototype 本质上是实例化对象上的

```js
Car.prototype.name = 'Benz'
function Car(){}
var car = new Car()
Car.prototype = {
    name:'Mazda'
}
console.log(car.name) // Benz
/*本质代码 在new的时候
	function Car(){
		var this = {
			__proto__:Car.prototype = {
				name:'Benz'
			}			
		}
	}
*/
实例化的时候本质上就是继承了构造函数的属性，实例化之后再修改prototype就和实例化对象无关了
```

- 插件写法

```js
(function(){
	var a = 1;
    function Test(){}
    window.Test = Test
})()
var test = new Test()
```

- 写一个插件,任意传两个数字，调用插件内部方法可进行加减乘除功能

```js
(function(){
    var Compute = function(opt){
        this.x = opt[0]
        this.y = opt[1]
    }
    Compute.prototype = {
        plus:function(){
            return this.x + this.y
        },
        minus:function(){
            return this.x - this.y
        },
        mul:function(){
            return this.x * this.y
        },
        div:function(){
            return this.x / this.y
        }
    }
    window.Compute = Compute
})()
```

- 原型链上的继承关系

```js
Professor.prototype.tSkill = 'JAVA'
function Professor(){}
var professor = new Professor()
Teacher.prototype = new Professor
function Teacher(){
    this.mSkill = 'JS'
}
var teacher = new Teacher();

Student.prototype = teacher
function Student(){
    this.pSkill = 'HTML'
}
var student = new Student()
console.log(student.pSkill) // JAVA
```

- 原型链的顶端是Object.prototype

- Obeject.create(对象,null) 创建自定义原型

```js
function Obj(){}
Obj.prototype.num = 1
var obj1 = Object.create(Obj.prototype)
var obj2 = new Obj()
console.log(obj1)
console.log(obj2) //两种形式完全一样	
```

- 利用create继承其他对象的原型

```js
var obj1  = Object.create(null)
console.log(obj1) // 未赋值前就是一个空对象{} 没有任何属性
obj1.num = 1
var obj2 = Object.create(obj1)
console.log(obj2)
console.log(obj2.num)
```

- 原型方法的重写
  - Number,boolean,String 都会重写部分Object原型里的方法

```js
Number.prototype.toString.call(1) // "1"
Object.prototype.toString.call(1) // "[object Number]"
```

### call和apply

- call和apply均可更改this指向，apply第二个参数为一个数组，但被改变的实例其本身含有的不会变

```js
function Car(brand, color){
    this.brand = brand
    this.color = color
}
var newCar = {}
Car.call(newCar, 'Benz', 'red')
Car.apply(newCar, ['Benz', 'red'])
console.log(newCar) //{brand: 'Benz', color: 'red'}
```

- 案例

```js
function Compute(){
    this.plus = function(a,b){
        console.log(a + b)
    }
    this.minus = function(a,b){
        console.log(a - b)
    }
}
function FullCompute(){
    Compute.apply(this)
    this.mul = function(a,b){
        console.log(a * b)
    }
    this.div = function(a,b){
        console.log(a / b)
    }
}
var compute = new FullCompute()
compute.plus)(1,2) // 3
```

- 案例

```js
function Car(brand, color, displacement){
    this.brand = brand
    this.color = color
    this.displacement = displacement
    this.info = function(){
        return '排量为' + this.displacement + '的' + this.color + this.brand
    }
}
function Person(opt){
    Car.apply(this, [opt.brand, opt.color, opt.displacement])
    this.name = opt.name
    this.age = opt.age
    this.say = function(){
        console.log('年龄' + this.age + '岁姓名为' + this.name + '买了一辆' + this.info())
    }
}
// Person构造函数借用Car构造函数里的方法和属性来实现买车功能
var p = new Person({
    brand:'奔驰',
    color:'红色',
    displacement:'3.0',
    name:'张三',
    age:'25'
})
p.say()
// 年龄25岁姓名为张三买了一辆排量为3.0的红色奔驰
```



### 继承

- 一个简单的例子

```js
Professor.prototype = {
    name:'Mr.Zhang',
    tSkill:'JAVA'
}
function Professor(){}
var professor = new Professor()
console.log(professor)
/*
	Professor {}
	[[Prototype]]: Object
	name: "Mr.Zhang"
	tSkill: "JAVA"
	[[Prototype]]: Object
*/
Teacher.prototype = professor
function Teacher(){
    this.name = 'Mr.wang'
    this.mSkill = 'JS/JQ'
}
var teacher = new Teacher()
console.log(teacher)
/*
	Teacher {name: 'Mr.wang', mSkill: 'JS/JQ'}
	mSkill: "JS/JQ"
	name: "Mr.wang"
	[[Prototype]]: Object
	[[Prototype]]: Object
	name: "Mr.Zhang"
	tSkill: "JAVA"
	[[Prototype]]: Object
*/

/*
	把professor的实例给了Teacher的原型，实现了继承(顺着__proto__一路找)
*/
```

- 企业级继承方案（圣杯模式）

```js
function Teacher(){
	this.name = 'Mr.Li'
    this.tSkill = 'JAVA'
}
Teacher.prototype = {
    pSkill:'JS/JQ'
}
var t = new Teacher()
console.log(t)
function Student(){
    this.name = 'Mr.Wang'
}
function Buffer(){}
Buffer.prototype = Teacher.prototype
var buffer = new Buffer()
Student.prototype = buffer
Student.prototype.age = 18
var s = new Student()
console.log(s)
// 其实就是弄一个不用的对象 做缓冲，就算改 也是改到缓冲的对象，不影响其他
// buffer实例化对象继承了Teacher 而student的prototype指向了buffer对象，当对student.prototype做修改时不影响Teacher
```

-  封装继承函数

```js
function Teacher(){}
function Student(){}
function Buffer(){}
inherit(Student, Teacher)
var s = new Student()
var t = new Teacher()

function inherit(Target, Origin){
    function Buffer(){}
    Buffer.prototype = Origin.prototype
    Target.prototype = new Buffer()
    Target.prototype.constructor = Target
    Target.prototype.super_class = 	Origin
}

// 最终版 模块化开发
var inherit = (function(){
    var Buffer = function(){}
    return function(Target,Origin){
        Buffer.prototype = Origin.prototype
        Target.prototype = new Buffer()
        Target.prototype.constructor = Target
        Target.prototype.super_class = Origin
    }
})()
function inherit(Target, Origin){
    function Buffer(){}
    Buffer.prototype = Origin.prototype
    Target.prototype = new Buffer()
    Target.prototype.constructor = Target
    Target.prototype.super_class = 	Origin
}
```

### 链式调用

```js
var sched = {
    a:function(){
        console.log(1)
        return this
    },
    b:function(){
        console.log(2)
        return this
    },
    c:function(){
        console.log(3)
        return this
    },
    d:function(){
        console.log(4)
        return this
    },        
}
sched.a().b().c().d()// 1 2 3 4
```

### 对象属性与遍历

- 取对象的值

```js
// 常规方法
obj.key
// 老方法
obj['key']
// 内部原理
obj.key -> obj['key']

// 例子
var myLang = {
    No1:'html',
    No2:'CSS',
    No3:'js',
    myStudyingLang:function(num){
        console.log(this.No + num) // NaN 这种取不到，原理如上
        console.log(this['No'+ num]) // 这样可以正常取到
    }
}
```

- 遍历(枚举)

```js
var car = {
	brand: 'Benz',
    color: 'red',
    displacement: '3.0',
    lang: '5',
   width: '2.5'
}
for(var key in car){
    // car.key是行不通的，内部是car['key']，结果是undefined
    console.log(key + ':' + car[key])
}
```

- hasOwnProperty  判断是否是自己的属性，过滤掉原型上的属性

- 安全的判断类型方法,不能判断自定义的类 object.prototype.toString.call()

```js
var a = []
console.log(Object.prototype.toString.call(a)) // '[object Array]'
```

### this指向

- 函数内部的this
  - 函数内部的this默认指向window

```js
function test(){
    this.d = 3
}
test()
console.log(this.d) // 3
```

- 构造函数的this

  - 经历了一个this对象变化的过程

  - this = window

  - this = {}

  - this = {\__proto__:Test.prototype,

    ​	key:value

    }

```js
function Test(){
    this.name = '123'
}
var test = new Test()
AO ={
    this:window -> {} -> {__proto__:Test.prototype,name:'123'}
}
GO = {
    Test:function(){}
    test: {...}
}
```

### caller/callee

- callee **指向拥有这个arguments对象的函数**

```js
function test(a,b,c){
    console.log(arguments.callee.length)//3
    console.log(test.length)//3
	console.log(arguments.length)//2
}
test(1,2)
```

- 例子

```js
var sum = (function(n){
	if(n <= 1) return 1
    return n + arguments.callee(n-1)
})(100)
// 立即执行函数求递归
```

- caller **保存着调用当前函数的函数的引用**

- 例子

```js
function outer() {
    inner();
}
function inner() {
    console.log(inner.caller)
}
outer();
//ƒ outer() {
//    inner();
//}
```

- 题目

```js
function foo(){
	bar.apply(null, arguments) // 相当于bar(arguments)
}
function bar(){
    console.log(arguments)
}
foo(1,2,3)
 // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

- 题目

``` js
undefined == null // true
undefined === null // false
isNaN('100') //false 存在隐式类型转化
parseInt('1a') == 1 // true 遇到非数字就停止
NaN == NaN // false NaN 不等于任何东西
{} == {} // false 引用值比较的是地址
```

- 构造函数里自定义return 会被覆盖

### new的实现

- 首先创建一个新的空对象

  然后将空对象的 `__proto__` 指向构造函数的原型

  - 它将新生成的对象的 `__prop__` 属性赋值为构造函数的 `prototype` 属性，使得通过构造函数创建的所有对象可以共享相同的原型。
  - 这意味着同一个构造函数创建的所有对象都继承自一个相同的对象，因此它们都是同一个类的对象。

  改变 `this` 的指向，指向空对象

  对构造函数的返回值做判断，然后返回对应的值

  - 一般是返回第一步创建的空对象；
  - 但是当 **构造函数有返回值时** 则需要做判断再返回对应的值，是 **对象类型则返回该对象**，是 **原始类型则返回第一步创建的空对象**。

```js
function myNew(Con,...args){
    // 创建一个新的空对象
    let obj = {}
    // 将这个空对象的__proto__指向构造函数的原型
    // obj.__proto__ = Con.prototype
    Object.setPrototypeOf(obj,Con.prototype)
    // 将this指向空对象
    let res = Con.apply(obj, args)
    // 对构造函数返回值做判断，然后返回对应的值
    return res instanceof Object ? res : obj
}
```

- 题目

```js
var a = 5
function test(){
    a = 0;
    console.log(a)
    console.log(this)
    console.log(this.a)
    var a
    console.log(a)
}
test()
/*
	0
	Window {0: Window, 1: Window, 2: Window, 3: Window, window: Window, self: Window, 		document: document, name: '', location: Location, …}
	5
	0
*/
new test()
/*
	0
	test {}
	undefined
	0
*/
```

### 对象克隆

- 问题引出

```js
var person1 = {
    name:'张三',
    age:18,
    sex:'male',
    height:100,
    weight:140
}
var person2 = person1
person2.name = '李四'
console.log(person1.name) //李四
// 把原来的值给改变了，实际上person2和person1指向的是同一个内存地址
```

- 浅拷贝(只涉及到第一层的属性)

```js
Object.prototype.named = '陈'
function clone(origin,target){
	for(var key in person1){
        target[key] = origin[key]
    }
}

var person1 = {
    name:'张三',
    age:18,
    sex:'male',
    son:{
        name:'李四'
    },
    arr:[]
}
var person2 = {}
clone(person1,person2)
person1.son.name = '123'
console.log(person2)
/*
age: 18
arr: []
name: "张三"
named: "陈"
sex: "male"
son: {name: '123'}
*/
// son仍然被改变而且原型上的属性也跟着赋给person2，这就是浅拷贝的问题
// 优化版本浅拷贝
function clone(origin, target){
    var tar = target || {}
    for(var key in origin){
        if(origin.hasOwnProperty(key)){
            tar[key] = origin[key]
        }
    }
    return tar
}
var person2 = clone(person1)
person2.name = 'lisi'
person2.son.forth = 'ben'
console.log(person1,person2)
```

- 深拷贝

```js
function deepClone(origin, target){
    var target = target || {},
        toStr = Obeject.prototype.toString,
        arrType = '[obejct Array]'
    for(var key in origin){
        if(origin.hasOwnProperty(key)){
            if(typeof(origin[key]) === 'object' && origin[key] !== null){
                if(toStr.call(origin[key]) === arrType){
                    target[key] = []
                }else{
                    target[key] = {}
                }
                deepClone(origin[key],target[key])
            }else{
                target[key] = origin[key]
            }
        }
    }
    return target
}
```

### 数组

- push与unshift
  - 返回值都是方法执行完之后数组的长度

- 封装自己的push方法

```js
Array.prototype.myPush = function(){
	for(var i = 0; i < arguments.length; i ++){
		this[this.length] = arguments[i]
	}
    return this.length
}
```

- splice方法

  - 接收三个参数
    - 开始项的下标
    - 删除长度
    - 替代的数据
  - 当下标为负数时代表从数组末尾开始数起

- sort()方法

  - 按照元素的ASCII码来进行排序的

  - 使用方法

    - arr.sort((a,b) => {a - b})  升序
    - arr.sort((a,b) => {b - a}) 倒序

  - 原理

  - ```js
    arr.sort((a,b) => {
        // 返回的是负值,a就排前面
        // 返回的是正值，b就排前面
        // 返回的是0，就保持不动
        return a > b ? 1 : -1
    })
    ```

  - 随机排序

    - ```js
      arr.sort((a,b) => {
      	return Math.random() - 0.5 > 0 ? 1 : -1
      })
      ```

      