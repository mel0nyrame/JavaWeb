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

可以使用set,get方法和实例/静态方法

使用#号创建私有变量(若直接使用同名的变量,则js会自动创建一个非私有的变量)

```js
class people{
    #n;

    get (n) {
        this.n = n;
    }
}
People people = new People();

console.log(people.n); //这里会创建一个新的变量
```

### 对象的浅拷贝和深拷贝

- 浅拷贝

```js
let arr = [1,2,3];
arr2 = arr;
console.log(arr2); //输出1,2,3
```

>浅拷贝只是将对象的地址赋值给新的的一个变量,若原始对象发生了变化,则拷贝后的对象也会发生改变

- 深拷贝

1.结构表达式,将元素赋值进去
2.将对象解析成字符串,再解析成对象

## 模块化处理

类似Java中的utils文件夹

- ES6模块化导入和导出方式
1.分别导出
2.统一导出
3.默认导出

>ES6无论以什么方式导出都是以对象方式导出,内容是对象的属性或者方法

- 分别暴露、导入

  - `module.js`
  - `app.js`

```js
const PI = 3.14
expert sqrt2 = 1.414; //导出关键词:expert
```

```js
import * as module from './module.js'
console.log(module.sqrt2); //只有这个属性
```

- 统一暴露、导入

  - `module.js`
  - `app.js`

```js
const PI = 3.14

class People {
    name;
    age;
}

expert{PI,People}
```

```js
import {PI,People} form './module.js'
```

- 默认导出
  - `module.js`

```js
const PI = 3.14

class People {
    name;
    age;
}

expert default PI; //只能有一个
```
