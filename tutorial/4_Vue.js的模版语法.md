# 模板语法

对于之前我们简单的说了说教程的计划，和开发中会使用的 IDE 编辑器。接下来我们来说说 Vue.js 的相关语法定义。

这里简单地说说语法这个知识点， 不管是任何的语言都会有一套语法结构， 能够让开发者使用起来很方便， 而且各个语言之间差异都不大。 而 Vue.js 则是基于 HTML 的模板语法进行实现的。 而一套成熟可用的语法结构， 一般都分为： 关键字，基础数据类型， 条件判断， 循环结构， 函数，事件等基础功能，而在前端的体系里， 又多了对于表单相关的语法定义。 而在最近这几年组件的兴起， 功能组件化也越来被越多的人拍手叫好。 对于组件的使用是贯穿整个 Vue.js 的开发周期， 所以学习一门新的语言和知识， 最主要的一开始， 要学习的就是这些基础点， 然后在这个基础之上， 才是你发挥的场地。

# Mustache

Vue.js 是基于 Mustache 的， 而 Mustache 是一款 logic-less 的前端模板引擎。最主要使用它的原因是规范和经典（大家都在用）。最基本的就是定义一个变量 {{ }} 双花括号的定义方式。然后在实际的 HTML 定义中 Vue.js 又实现了一系列的指令，就是响应 标签的属性中的自定义属性。 以 v-* 来定义。 这个就是后端学习中模板语法的基础关键字。死记硬背也要记牢的。当然以人类的记忆遗忘曲线， 你天天用它也就记住了它。 一开始就差文档就好了， 收藏我这个文章， 后续可以当文档用哦！

# 语法部分思维导图

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632540062066-314462c6-516d-44ef-84df-43fb8166c42e.png)





# 准备开始

这里先说怎么简单使用， 后续将针对每一个标签都书写一篇教程， 尽量能涵盖所有的知识点。 

这里使用方式用两种进行讲解。 一个是普通的引用方式进行， 另外一个是单体应用。讲解的话直接引用更能说明基础的语法关系。 

所有编程语言第一个案例都是 hello world 程序。这里也不例外。

打开我们的开发工具， 这里下用 VS Code 开开胃。

## 创建应用

创建一个hello.html文件， 并编写如下代码。 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632418167805-998a0fe7-7453-4447-80ff-5de84716bad0.png)

按下F5, 或者点击菜单 运行 -> 调试  选择 Chrome， 如果没有， 就先下载吧。 后续基本以它为主了。![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632418292259-0c272ae2-5f2f-4217-ab4e-2a1875419625.png)

启动调试之后， 我们可以看到浏览器的效果。

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632418383830-a8216482-34c9-4688-baac-5f822907b2d9.png)

就这样我们就完成了 Vue.js 的使用。 学习起来十分简单， 只要有 HTML 和 JavaScript 的一点基础知识， 就可以很顺手的使用。 代码很简单， 我们过一下这个流程。 

### 首先第一步， 引入框架

你要使用一个框架， 不管是什么类型的语言， 都会有对应的引用方式， 比方说 Java 的 jar, 正确的引入了才能使用它，否则只会给你说你在干什么？ 

而 HTML 应引入一个 JavaScript 的框架， 所以就需要 script 标签进行引入使用。 

```
<script src="https://unpkg.com/vue@next"></script>
```

这里暂时使用 CDN 的引用方式， 后期研究代码执行步骤， 就需要考虑使用本地的方式进行查看， 这样更容易分析代码。

### 第二步， 声明一个模板

```
const HelloVueApp = {
  data() {
    return {
      message: "Hello Vue.js!!",
    };
  },
};
```

这是一个最简单的使用方式， 这里把变量都放到一个函数 data() 中， 这里放到 data 函数， 而不是放到 data 属性中， 因为以下的原因。 

1. 防止 data 中的变量出现数据污染。不使用函数包裹的数据将会在全局可访问。

```
var Vue = function () {};
Vue.prototype.data = { a: 0, b: 1 };
var var1 = new Vue();
var var2 = new Vue();
var1.data.a = 3;
console.log(var2.data.a); // 3
//直接暴露属性会造成数据污染， 因为共用一个数据。
```

1. 组件实例中的 data 必须为函数， 为了防止多个组件之间调用共用一个 data, 产生数据污染。

```
var Vue = function () {
  this.data = this.dataFun();
};
Vue.prototype.dataFun = function () {
  return {
    a: 1,
    b: 2,
  };
};
var v1 = new Vue();
var v2 = new Vue();
v1.data.a = 5;
console.log(v1.data.a, v2.data.a); //5 1
```



1. 在创建实例的过程中， 该函数返回的是一个对象， 这样可以通过 Vue 的响应式系统将其封装起来， 并以 $data 的形式存储在组件实例中。并且该对象的任何顶级的属性（property）也会通过组件实例暴露出来。并且该函数中的属性都可以使用 Mustache 语法进行访问，也就是页面中 {{ message }} ， 改造上面的代码就可以访问 message 变量

```
const vm = Vue.createApp(HelloVueApp).mount("#hello-vue");
console.log(vm.$data.message) // 控制台输出 Hello Vue.js!!
```

当然除了 data 之外还有： methods，props, computed， watch，provide，inject, setup，emits 等。

然后使用 Vue 暴露出来的 createApp 进行创建。 使用 mount 绑定一个页面元素。 这里使用 id # 的方式关联到 hello-vue 的 id 属性上。 最终我们就看都了页面中的效果。 

### 最后， 渲染到页面上

```
{{ message }}
```

整个过程都在 Vue.js 的构建生命周期中。 因为每个组件创建的过程中都要经过一系列的初始化的过程， 比方说： 监听数据， 编译模板， 将实例挂载到 DOM 上， 并响应监听数据的变化更新到 DOM上等。在这个生命周期就会定义一些钩子函数，可以让使用者在不同的阶段添加代码的机会。

## Vue.js 的生命周期图示

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632423413556-f2c5254d-0e19-46dd-a6b5-a12ee70f9fe9.png)

### created 在实例创建完成后被同步调用。

单独说这个钩子函数， 后续所有的周期钩子都会详细的介绍， 现在先简单的收下 created, 也就是说， 你可以认为这个函数执行的时候， 页面基本上已经加载完毕。 在 data 下写上下面代码， 执行查看效果， 你会发现， 执行的步骤。

```
beforeCreate(){
	console.log("beforeCreate"); 
},
created(){
	console.log("created");
},
```

## 模板中使用指令

指令 (Directives) 是带有 v- 前缀的特殊 attribute。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。对于指令大部分都是操作的单个 JavaScript 表达式， v-for 和 v-on 是例外情况， 会在后面循环和事件中， 详细说明。

### v-text

我们来看一个指令  v-text， 将一个变量中的数据填充到标签中。

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632424782540-525103e4-43c3-4d44-826f-b39c5d53f3d8.png)

浏览器查看效果： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632424890333-c335e56e-ae9d-4a17-a0c7-c7ebdccff70b.png)

从这里可以看出来和 {{}} 模板语法起到相同的作用， 就是让数据放入到标签中显示出来。 

然后我们来用一些其他的使用方式， 加载不同类型的数据。

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632425527801-da67a550-ac78-49a1-8ec9-21040de924f0.png)

先看效果： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632425567792-0ba7042f-df54-439f-829f-a56d3ee9bf9c.png)

除了基础的变量类型， 我们可以使用对象，也可以使用数组， 还可以使用简单的三目运算， 并且还可以进行赋值的操作。这里只能是单个表达式。 页面中继续加入如下代码：

```
<div v-text="message = '赋值可行?'"></div>
```

刷新页面， 我们会发现， 页面中 message 变量中的数据已经变成 “赋值可行？”这里需要注意的是， 能赋值的是之前就创建好的对象， 如果该对象或者变量不存在就不允许使用了。



当然我们也可以使用一些受限的 JavaScript 函数。 例如： Date 函数。 继续加入以下代码：

```
<div v-text="new Date().getYear() + 1900"></div>
```

查看效果， 我们发现页面中打印了 2021 的年份信息。因为这是模板引擎， 所以实现调用原始函数有限， 仅限于如下函数可以使用： 

```
Infinity,undefined,NaN,isFinite,isNaN,parseFloat,parseInt,
decodeURI,decodeURIComponent,encodeURI,encodeURIComponent,Math,Number,Date,Array,
Number,Date,Array,Object,Boolean,String,RegExp,Map,Set,JSON,Intl,BigInt'
```

根据以上的案例我们可以得出 {{}} 模板应该也和 v-text 一样的效果。 我们继续改造我们的代码。 

```
<div>
	当前： {{ new Date().getYear() + 1900 }}年
</div>
```

可以看到页面中显示出来了对应的效果。 当前： 2021年， 如果你能出现 2050年，可能这个框架应该已经不在出现， 又有新的技术将它们代替。你只是在考古， 考的是我曾经遗留下来的代码笔记。

v-text的最终代码和效果图如下图所示： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632426437496-4baa71d9-0a4e-430b-95b2-78c4e2972e5d.png)

效果图：

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632426443829-d7048299-8099-427b-b19d-a444c6120ed7.png)

### v-once

使用 v-once 执行一次性的插值操作， ， 他只渲染元素和组件进行一次处理， 后续又数据的调整将不再修改数据。这可以优化代码的性能， 代码如下： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632426924346-494d7c8f-4473-4291-a3d2-15aaab6f081e.png)

效果图：

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632427049050-bab3d077-0c41-452d-b1dc-b27abfbc7b04.png)

这里我们发现， 原始的数据并没有进行数据的响应， 而后续的正常使用插值指令的就进行数据更新。这里需要注意的是， v-once 的整个标签作用域内，所有的数据都不会发生变化。

### v-cloak

使用 v-cloak 解决 Vue 还没有实例化， 模板语法 {{}} 显示到页面中的问题， 该问题只会出现在非组件化的或者单页的模式下，该指令主要是保持元素上的组件在实例化结束以后才能显示， 可以结合` [v-cloak] { display: none } `一起使用时起到隐藏 Mustache 标签的作用， 具体代码如下： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632457243857-24392814-472d-4c12-8abc-bd747d905ed3.png)

该案例的效果通过只有在访问速度过慢的时候才能显示出来。 

### v-show

根据表达式的真假值，切换 HTML 元素的 CSS display 状态， 进行隐藏和显示元素信息， 这里元素不管是否显示都会加载到页面上。当切换频率很高的时候， 推荐使用该方式， 当切换频率不高的情况下可以选择该方式， 或者使用 v-if 指令。 演示代码如下:

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632469454117-762d1a4b-59bb-42e6-88bb-0c86c635b4f4.png)

效果如下： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632469489323-80e4c733-b7b5-40ad-8bac-b1e975b29fba.png)

### v-html

使用 innerHTML 更新元素的内容， 保留元素的格式， 这里内容只会按照普通的 HTML 插入， 并不会作为 Vue 的模板进行编译。如果真的要使用 v-html 的组合模板， 可以考虑使用组件来代替， 这里可以使用在富文本内容的显示上。代码案例如下。

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632470760866-717498e3-acff-490f-b2f3-5cdd8666b68c.png)

效果图如下 : 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632470782226-015094ce-8764-45bc-ae97-111817347afe.png)

### v-pre

不需要编译器去编译表达式，用来显示原始的元素数据， 这里可以写在没有指令的元素节点上， 加快编译的速度。演示效果， 代码如下： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632471292119-758f7989-1ce5-4109-9d89-43df0bac0326.png)

效果图如下： 

![img](https://cdn.jsdelivr.net/gh/xymiao/xymiaocdn/res/2021/202110/1632471312929-4cb3f744-1069-486a-9bb2-907b2324786b.png)

### v-on

该指令可以使用 @ 进行缩写。 绑定页面中的事件， 也就是一个事件监听器。 该表达式可以是一个函数的名字， 也可以是一个内联语句， 当然也可以绑定一个修饰符，没有修饰符的情况下可以省略。 这里只做说明， 后续事件详细讲解这个属性。 上面显示和隐藏元素 v-show 的时候， 演示过该用法。 

```
<button v-on:click="doThis"></button>
//doThis 可以是方法名， 或者内联语句。
```

### v-bind

该指令可以使用 ： 进行缩写。 绑定一个或者多个元素属性 （ attribute ）, 或者绑定一个组件 prop 到表达式。 

```
<!-- 绑定 attribute -->
<img v-bind:src="imageSrc" />

<!-- 动态 attribute 名 -->
<button v-bind:[key]="value"></button>

<!-- 缩写 -->
<img :src="imageSrc" />

<!-- 动态 attribute 名缩写 -->
<button :[key]="value"></button>
```



本次基础模板和指令的使用先告一段落， 接下来我们来看一下一个模板中必有的条件判断。 关注我。 持续关注后续内容。