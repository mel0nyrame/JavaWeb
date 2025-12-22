# ES6

## 变量定义符号

let与var区别

- let不能重复声明

- let在括号内变成局部变量

- let不会预解析

- let定义的全局变量不会作为window的属性

- const为let的常量

## 拼接字符串

可使用\``符号包裹多行字符串,使用\${var}即可使用变量

## 解构表达式

### 数组解构复制

按照顺序获取变量

```js
let [a,b,c] = [1,2,3]; // a = 1,b = 2,c = 3
```

### 对象属性解构

```js
let person = {
    name:"zhangsan",
    age:18
}

let {name,age} = person; //变量名一定要和对象中的属性名相同
```

## 箭头函数

类似lambda(几乎一模一样)

```js
let fun1 = function(){}; //普通表达式
let fun2 = () => {}; //箭头函数
```

>箭头函数中的this是外部的变量,非成员变量

## rest和spread

### rest

```js
//相当于可变参数,处理abc外的参数都放假arr里
let fun1 = (a,b,c,...arr) => {
    console.log(a,b,c);
    console.log(arr);
}
```

### spread

```js
let arr = [1,2,3];
let fun1 = (a,b,c) => {
    console.log(a,b,c);
}

fun1(...arr); //作为实参调用,将123分别赋值给abc,使用...参数名代替多个参数
```

## 对象

>ES6中新增了class,extends,constructor关键词

### 语法糖

