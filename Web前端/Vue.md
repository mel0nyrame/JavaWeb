# Vue

核心功能:

- 声明式渲染:基于标准HTML扩展的一套模板语法
- 响应性:Vue会自动追踪js状态并实时更新DOM

## 快速体验

```js
<script>
    const app = new Vue.createApp({
        setup(){
            
        }

        return {

        }
    })
<script>
```

在setup中定义变量,在return中返回,再使用Vue命令绑定相对于的元素就可以与HTML关联了

## Vite

用来创建和打包一个基础的满足Vue3的Project

创建项目命令

```bash
npm create vite@latest
npm install //进入创建好的文件夹内执行
npm run dev //启动项目
```

## Vue3目录

- `public/`：公共资源（HTML、图像、字体），直接复制到构建目录

- `src/`：源代码，由Vite实时编译

### src/ 内部划分

1. `assets/`：静态资源（图片、字体、样式）
2. `components/`：可复用的UI组件
3. `layouts/`：布局组件（头部、底部、导航）
4. `pages/`：页面级组件，对应路由
5. `plugins/`：Vite插件配置
6. `router/`：Vue路由配置
7. `store/`：Vuex状态管理
8. `utils/`：通用工具函数

- `vite.config.js`：Vite配置文件

- `package.json`：Node.js项目配置

- `src/main.js`：项目入口文件

## SFC入门

SFC:单文件组件,传统页面有html,css,js文件,Vue将他们封装成一个文件(Single-File Component),分别由`<template>`,`<style>`,`<script>`标签组成

多个页面可能存在相同的部分,复用率极高,于是将他们分离出来,做成组件,于是页面就变成了由各个组件组成的页面

项目过程:

```mermaid
graph LR
A(index.html) --> B(main.js) --> C(App.vue) -->D(其他components)
```

1.在index中引入main.js
2.在main.js初始化App对象
3.在App.vue中引入其他组件,通过标签使用组件,例:

```js
<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <HelloWorld msg="Vite + Vue" />
</template>
```

css导入方式:

- 在`<script>`标签中导入
- 在`<script>`标签中导入,但是要加@(@import)
- 在main.js中导入,作用到全局

## Vue3语法

### setup函数和响应式数据

在`<script>`标签后面加上setup,即可省略return等复杂的语法结构

使用vue的ref()函数可将变量变为响应式函数

```js
<script setup>
    import {ref} from 'vue'

    let counter = ref{0} //相当于创建了一个对象,在<script>标签中使用该变量都要加.value才能对此变量进行操作
    
    function add() {
        counter.value++
    }
</script>

<template>
    <div>
        <span v-text="counter"></span> //在<template>标签中不用加.value
    </div>
</template>

```

### 插值表达式

```js
<script>
    let PI = 3.14
</script>

<template>
    <h1>{{ PI }}</h1>
```

特点:

- 可将数据绑定到元素上面
- 可将函数的返回值渲染到指定位置
- 支持一些运算符({{ age > 18 ? '是' : '否' }})
- 支持调用api({{ str.reverse() }})

### Vue指令

- v-xxx:vue的指令
  - 要求:
    - 命令必须依赖标签,放在开始标签中
  - v-text:用来渲染文本(v-text="你好")
    - 支持模板字符串(v-text="\`你好 ${hello}\`")
    - 支持常见api调用
    - 支持调用函数
    - **不支持**html格式
  - v-html:用来渲染html格式(v-html="\<h1>你好\<h1>")
    - 专门用来渲染html格式
  - v-bind:将数据绑定到元素的属性上(v-bind:属性="")
  - v-on:监听DOM操作(v-on:click=""或者@click)
    - 在原生js中的onClick之类的操作要吧on去除
    - 内联事件:`<button v-on:click="counter++" >+</button>`
    - .once事件只绑定一次:`<button v-on.once:click="counter++" >+</button>`
    - .prevent事件取消默认行为(例如\<a>可以跳转连接,加入修饰符可以阻止)

### 响应式数据处理

变量定义:

```js
<script setup>
    import {ref} from 'vue'

    let counter = ref{0}
</script>
```

对象定义:

```js
<script setup>
    import {reactive} from 'vue'

    let people = reactive(
        name:"",
        age:""
    )
</script>
```

- toRef():将reactive中的某个数据转换成ref数据
- toRefs():将reactive中多个数据转换成ref数据(属性名要相同)

### 条件渲染

- v-if:在某些条件下渲染对应元素(\<h1 v-if="true">Hello\<h1>)
  - v-else:当v-if为false时自动渲染
    - 需要和v-if搭配
- v-show:和v-if使用方法相同,但v-if的条件为false时不会被渲染,v-show无论怎么样都会被渲染但是通过css样式隐藏
  - v-if有更多的切换开销,v-show有更多的渲染开销

### 列表渲染

- v-for:用来渲染列表(迭代,也能添加响应式数据)

```js
<script setup>
import { reactive } from 'vue';

let person = reactive({
  person1 :{
    name:"xiaoming",
    age:"18"
  },
  person2 :{
    name:"xiaohong",
    age:"18"
  },
  person3 :{
    name:"xiaoqing",
    age:"18"
  },
  person4 :{
    name:"xiaofang",
    age:"18"
  }
})
</script>
<template>
  <div id="app">
    <ul>
      <li v-for="personit in person">
        {{ personit }} //自动渲染出集合里面的元素
      </li>
    </ul>
    
  </div>
</template>
```

### 双向绑定

- 单向绑定(v-bind):当用户的操作对响应式数据进行了修改的时候只会更新DOM树,不会更新响应式数据
- 双向绑定(v-model):当用户当用户的操作对响应式数据进行了修改的时候不仅会更新DOM树,也会更新响应式数据

### 属性计算

`computed()`函数:计算属性

>若computed()函数中的响应式数据没发生改变,不管调用多少次都是渲染上次的结果,效果相当于优化,而使用普通的函数每刷新一次页面都会加载一次

```js
let num1 = ref(2)
let num2 = ref(3)

let value = computed(() => {
  num1 > num2 ? "否":"是"
}) //箭头函数
```

### 数据监听器

应用场景:

- 当数据发生变化时需执行相对应的操作
- 监听数据变化,当满足一定条件之后触发操作
- 在异步操作前或操作后执行相对于的操作

不同函数

- watch()

>value为需要监听的响应式数据,newValue是变化后的值,old是变化前的值,deep是是否执行深度搜索的选项,immediate是是否立即渲染

```js
//监听ref
watch(value,(newValue,oldValue) => {
})

//监听一个reactive
watch(() => object.value,(newValue,oldValue) => {
})

//监听多个reactive
watch(() => object.value,(newValue,oldValue) => {
},{deep=true,immediate:true})
```

- watchEffect()

>当任何一个数据发生变化时候都执行,无传入参数

### Vue生命周期

在初始化Vue组件的时候会运行一些配置,其中就会运行生命周期的钩子函数,我们便可以在钩子函数添加自己的代码

![alt text](../Web前端/img/image-4.png)

>红色的为钩子函数

详细的看VueApi(https://cn.vuejs.org/guide/essentials/lifecycle.html)

重写生命周期函数

```js
import { onMounted,onActivated,onBeforeMount,onBeforeUnmount,onBeforeUpdate,onUpdated } from 'vue';

//重写方式
onMounted(() => {
})
```

### Vue组件(SFC)拼接页面

将页面分成几个部分,然后每个部分都用vue文件替代,再写css调整位置

### Vue组件参数传递

- 父传子(组件)
- 子传父(组件)
- 兄弟互传(组件)
  - 需要先传给父组件再传给兄弟组件

在后面的router或pinia中会详细地提及
