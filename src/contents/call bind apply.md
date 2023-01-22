---
author: isolcat
datetime: 2022-12-2T04:58:53Z
title: call bind apply
featured: false
draft: false
tags:
  - 面试题
ogImage: ""
description: call bind apply的区别
---


# call bind apply

## call

函数调用了`call`方法，并且把this绑定到egg对象上，使得输出的时候就可以用上对象的属性name了

> call就是用来修改**this的指向**，call也可以调用函数

```js
function person(){
	console.log(this.name)
}
var egg={
    name:'蛋老师'
}
person.call(egg)
//“蛋老师"
```

call可以传多个参数，第一个参数是**要改变的this的指向**，往后的参数是往里面**传参**

```js
let cat={
    name:"喵喵"
}
let dog={
    name:"旺财",
    sayName(){
        console.log("我是"+this.name)
    }
    eat(food){
        cosnole.log("我喜欢吃"+food)
    }
}
dog.eat.call(cat,"鱼")
//我喜欢吃鱼
dog.eat.call(cat,"鱼","肉")
//我喜欢吃鱼肉
```



## bind

bind不会立即执行函数，传参的方式和`call`是一模一样的，但我们直接调用它是不会有作用的，bind**不会调用这个函数**，会将这个函数以**返回值的方式返回出来**

```js
let cat={
    name:"喵喵"
}
let dog={
    name:"旺财",
    sayName(){
        console.log("我是"+this.name)
    }
    eat(food){
        cosnole.log("我喜欢吃"+food)
    }
}
let fun=dog.eat.bind(cat,"鱼","肉")
fun()//我喜欢吃鱼肉
```

## apply

`apply`传参和call的最大区别是，apply是以**数组的形式**进行传参

```js
let cat={
    name:"喵喵"
}
let dog={
    name:"旺财",
    sayName(){
        console.log("我是"+this.name)
    }
    eat(food){
        cosnole.log("我喜欢吃"+food)
    }
}
dog.eat.call(cat,"鱼")
//我喜欢吃鱼
dog.eat.call(cat,"鱼","肉")
//我喜欢吃鱼肉

//apply传参和call的最大区别是，apply是以数组的形式进行传参
dog.eat.apply(cat,["鱼","肉"])
//我喜欢吃鱼肉
```

