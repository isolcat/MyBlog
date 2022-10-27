---
author: isolcat
datetime: 2022-0-29T15:20:35Z
title: Typescript学习笔记
slug: ""
featured: false
draft: false
tags:
  - Typescript
ogImage: ""
description:
  Typescript是js的超集
---

# Typescript笔记

## interface(接口)

interface是在JavaScript中不存在的概念，在ts当中，除了可用于对类的一部分行为进行抽象类实现接口)以外，也常用于对「对象的形状（Shape）」进行描述。

![image-20220427222708177](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220427222708177.png)

例如在此处定义了一个接口对象`Person`，那么变量的形状必须和其保持一致，多一个 少一个属性都不可以

### ?:(可选属性)

但有时候我们不确定是不是会用上这个属性，这时候就可以在属性名称的后面跟上一个`?`例如：![image-20220427222948934](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220427222948934.png)

此时的age属性便是可选的

### readonly（只读）

还有一部分的属性我们需要他**只读**，便可以使用`readonly`这一属性![image-20220427223103597](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220427223103597.png)

这时候该属性只能查看，无法进行修改

注意如果 `readonly` 和其他访问修饰符同时存在的话，需要写在其后面。

例子：

```typescript
class Animal {
  // public readonly name;
  public constructor(public readonly name) {
    // this.name = name;
  }
}
```



![image-20220427223152024](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220427223152024.png)

当我们给他赋值之后，便无法再进行修改了



## Function（函数）

![image-20220427232549810](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220427232549810.png)

此处是限制了输入和输出（*箭头指向的是输出*）

![image-20220427232811560](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220427232811560.png)

函数的可选和接口是一样的，都是在参数的后面添加`?`



![image-20220427232929860](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220427232929860.png)

此处的箭头不是es6中的箭头函数，而是ts中声明函数返回值的方法（*此处在声明返回的为number*）



## 类型推断

当没有明确声明类型的时候，ts会对其进行类型推断，来判断赋值是否符合类型正确

例如：![image-20220428181242936](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428181242936.png)



## 联合类型

由于`any`不到万不得已最好不使用，但有时候又确实需要使用该功能，这里便可以使用**联合类型**

![image-20220428181529696](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428181529696.png)

在对其类型进行下定义的时候，便可以在其中间加一个`|`让其拥有多种类型

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**：

```ts
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

上例中，`length` 不是 `string` 和 `number` 的共有属性，所以会报错。

访问 `string` 和 `number` 的共有属性是没问题的：

```typescript
function getString(something: string | number): string {
    return something.toString();
}
```



## 类型断言

类型断言可以用来指定一个值的类型

```typescript
值 as 类型
```

或

```typescript
<类型>值
```

在 tsx 语法（React 的 jsx 语法的 ts 版）中必须使用前者，即 `值 as 类型`。

形如 `<Foo>` 的语法在 tsx 中表示的是一个 `ReactNode`，在 ts 中除了表示类型断言之外，也可能是表示一个[泛型](http://ts.xcatliu.com/advanced/generics.html)。

故，在使用类型断言的时候，最好能使用`as`这种方式



## 枚举（enum）

枚举是ts的一个特点（*js没有的*）

1. 枚举默认从0开始赋值
2. 如果某个属性赋值，其他属性也要赋值，否则报错
3. js的赋值语句返回被赋的值
4. 定义枚举时加const则为常量枚举，常量枚举可提升性能

![image-20220428183116406](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428183116406.png)

在`enum`前面加一个`const`便被称为常量枚举

![image-20220428184916878](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428184916878.png)

此时再进行编译![image-20220428184939252](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428184939252.png)

便会得该结果![image-20220428185004702](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428185004702.png)

常量枚举会提升枚举的性能，让其变得更加清楚明了

但**不是**任何enum都可以使用常量枚举，只有常量值才能进行常量枚举，**计算值**无法进行常量枚举



## 泛型（Generics）

Generics是typescript最难的一部分

<img src="C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428185621591.png" alt="image-20220428185621591" style="zoom:33%;" />

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

在尖括号的内部（`< >`）定义一个泛型（名字无所谓 但常用T来表示）

![image-20220428190935272](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220428190935272.png)

```ts
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

上例中，我们在函数名后添加了 `<T>`，其中 `T` 用来指代任意输入的类型，在后面的输入 `value: T` 和输出 `Array<T>` 中即可使用了。

接着在调用的时候，可以指定它具体的类型为 `string`。当然，也可以不手动指定，而让类型推论自动推算出来：

```ts
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```



泛型约束 通过 **extends** 关键字来设置约束条件,而不是想传入啥就传入啥

泛型和数组联用：T[]

泛型和接口联用: 使用`extends`关键字继承接口

```typescript
function echoArr<T>(arg: T[]): T[] {
    console .log(arg.length)
    return arg
}
const aees =echoArr([1,2,3])
interface IWithLength {
    length: number
}
function echoWithLength<T extends IWithLength>(arg: T):T{
    console.log(arg.length)
    return arg
}

const str = echoWithLength('str')
const obj =echoWithLength({ length:10})
const arr2 =echoWithLength([1,2,3])
```

1. 泛型使用场景：1）函数的参数及返回值；2）泛型类； 3）接口使用泛型 ；4）泛型数组定义

2. 无论哪种使用，调用泛型定义的函数或类或接口时，都要手动赋值泛型类型(函数使用场景可省略)

3. ts内置原始类型可以使用泛型

4. ```typescript
   let arr: Array<number | string> = [1, '2']
   ```



## 类型别名（type aliase）

类型别名用来给一个类型起个新名字。

类型别名常用于联合类型。

```
 type PlusType=(x:number,y:number)=>number
 let sum3:PlusType;
 const result4=sum3(2,4);
```

## 字符串，字面量

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

```
 type StrOrNumber=string|number;
 let result5:StrOrNumber='123';
 //字面量指定类型
  const str0:'name'='name';
  const number:1=1
  type Directions='up'|'Down'|'left'|'Right';
  let toWhere:Directions='left'
```

## 交叉类型

用`&`来连接

```
interface IName{
  name:string
}
type Iperson=IName&{age:number}
let person:Iperson={name:'123',age:23}
```

![image-20220429211444233](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220429211444233.png)

## 内置对象

JavaScript 中有很多[内置对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)，它们可以直接在 TypeScript 中当做定义好了的类型。

内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

### ECMAScript 的内置对象

ECMAScript 标准提供的内置对象有：

`Boolean`、`Error`、`Date`、`RegExp` 等。

我们可以在 TypeScript 中将变量定义为这些类型：

```ts
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

更多的内置对象，可以查看 [MDN 的文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)。

而他们的定义文件，则在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib)中。

### DOM 和 BOM 的内置对象

DOM 和 BOM 提供的内置对象有：

`Document`、`HTMLElement`、`Event`、`NodeList` 等。

TypeScript 中会经常用到这些类型：

```ts
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

它们的定义文件同样在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib)中。

### TypeScript 核心库的定义文件

[TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib)中定义了所有浏览器环境需要用到的类型，并且是预置在 TypeScript 中的。

当你在使用一些常用的方法的时候，TypeScript 实际上已经帮你做了很多类型判断的工作了，比如：

```ts
Math.pow(10, '2');

// index.ts(1,14): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

上面的例子中，`Math.pow` 必须接受两个 `number` 类型的参数。事实上 `Math.pow` 的类型定义如下：

```ts
interface Math {
    /**
     * Returns the value of a base expression taken to a specified power.
     * @param x The base value of the expression.
     * @param y The exponent value of the expression.
     */
    pow(x: number, y: number): number;
}
```

再举一个 DOM 中的例子：

```ts
document.addEventListener('click', function(e) {
    console.log(e.targetCurrent);
});

// index.ts(2,17): error TS2339: Property 'targetCurrent' does not exist on type 'MouseEvent'.
```

上面的例子中，`addEventListener` 方法是在 TypeScript 核心库中定义的：

```ts
interface Document extends Node, GlobalEventHandlers, NodeSelector, DocumentEvent {
    addEventListener(type: string, listener: (ev: MouseEvent) => any, useCapture?: boolean): void;
}
```

所以 `e` 被推断成了 `MouseEvent`，而 `MouseEvent` 是没有 `targetCurrent` 属性的，所以报错了。

注意，TypeScript 核心库的定义中不包含 Node.js 部分。

### 用 TypeScript 写 Node.js

Node.js 不是内置对象的一部分，如果想用 TypeScript 写 Node.js，则需要引入第三方声明文件：

```bash
npm install @types/node --save-dev
```