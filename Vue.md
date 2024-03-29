#                                          											Vue2

------

参考笔记：[vue基础用法&基础原理整理_vue &-CSDN博客](https://blog.csdn.net/hangao233/article/details/123990192?spm=1001.2014.3001.5502)

## 概念

### vue

Vue 是一套用于构建用户界面的渐进式框架，vue可以设计自地向上的逐层应用，面对组件化，不需要关心全局。免除原生JavaScript中的DOM操作，简化书写。

**vue的特点：1.采用组件化的思想，提高代码复用率。**

​						**2.由命令式--->声明式（编码人员无需直接操作DOM）**

​						**3.使用虚拟DOM+优秀的Diff算法，尽量复用DOM节点**

![](https://cdn.jsdelivr.net/gh/Advance-Mr/image/b5c2d7bf7d23ac1b7ece761184b6ecb-1709099128361-13.png)
参考思想：基于MVVM(Model-View-ViewModel)思想，实现数据的双向绑定，将编程的关注点放在数据上。

![](C:\Users\Administrator\Desktop\R星\assets\image-20240226102235003.png)

M：模型/data中的数据

V：视图/模版

VM: 视图模型/vue实例对象

### node.js

是一个开源的能够跨平台的javaScript运行时环境.Node.js是一个基于Chrome V8引擎的JavaScript运行时环境。它让我们可以在服务器端运行JavaScript代码，实现更丰富的功能。Webpack依赖于Node.js环境，通过Node.js的模块系统来管理和加载项目中的各种资源。

### **Webpack** 	

webpack是一个**打包器（bundler）**，它能将多个js文件打包成一个文件（其实不止能打包js文件，也能打包其他类型的文件，比如css文件，json文件等）。能将前端页面众多的js,css等文件打包成一个文件。**Webpack** 是一个开源的JavaScript模块打包器，它可以将许多模块按照依赖关系打包成一个或多个文件。它在Node.js环境中运行，帮助开发者在构建大型项目时管理复杂的依赖关系。

## 常用指令

### v-bind:

单向绑定(v-bind)---数据只能从data流向页面

```js
v-bind 用来添加动态属性的，常见的 src、href、class、style、title 等属性都可以通过 v-bind 添加动态属性值
// 语法糖写法
<div :title="title">
 <img :src="url" alt="">
 <a :href="link">没有语法糖</a>
</div>
```

### v-model：

双向绑定数据

```js
v-model 绑定数据之后，既绑定了数据，又添加了事件监听
//语法糖写法
<input type="text" v-model="name" >
```

### v-on：

为html标签绑定事件

```js
v-on 绑定事件监听器的，v-on 的语法糖，就是简写成@ 
<button @click="btn">语法糖</button>

<button v-on:click="btn">无语法糖</button>
```

v-if、v-else\v-else-if：条件绑定

v-show：条件绑定

v-for:列表渲染

###  v-text :

 更新元素的 textContent

```js
<h2 v-text='label'> 文本一</h2> //v-text会替换标签中的文本内容，不会解析
data(){
    return{
    label:'我将替换文本一'
    }
}
```

### 键盘事件语法糖：

@keydown，@keyup

- 回车 => enter

- 删除 => delete

- 退出 => esc

- 空格 => space

- 换行 => tab (特殊，必须配合keydown去使用)

  
  
  ### **computed和watch之间的区别**
  
  - computed能完成的功能，watch都可以完成
  - watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作

## 生命周期

每执行一个生命周期，就会执行一个钩子函数

补充：异步请求。访问页面就会触发钩子函数-挂载。然后异步请求返回数据到数据模型emp中，通过vue渲染展示。

![](C:\Users\Administrator\Desktop\R星\assets\image-20240226122853200.png)

#### 记忆：

​		**Vue 实例的创建是在 Vue 的 `beforeCreate` 生命周期钩子函数中**。**完成是在mounted阶段。**Vue 实例的创建过程开始于 `beforeCreate` 生命周期钩子，这时实例初始化工作已完成，但还没有进行数据观测和事件配置。随后在 `created` 阶段，实例已经可以进行数据访问和方法调用，但尚未开始挂载到 DOM。

​		**created创建后**，完成数据（data、props、computed）的初始化导入依赖项。
​		可访问 data computed watch methods 上的方法和数据。

​		**Dom节点的创建是在mounted中。**

#### 第一次页面加载会触发哪几个钩子？

​		答：beforeCreate， created， beforeMount， mounted

#### 简述每个周期具体适合哪些场景

​		答：beforeCreate：在beforeCreate生命周期执行的时候，data和methods中的数据都还没有初始化。不能在这个阶段使用data中的数据和methods中的方法

​	created：data 和 methods都已经被初始化好了，如果要调用 methods 中的方法，或者操作 data 中的数据，最早在改阶段

​	beforeMount：执行到这个钩子的时候，内存中已经编译好了模板了，但是还没有挂载到页面中，此时，页面还是旧的

​	mounted：Vue实例已经初始化完成，页面成功渲染。 如果我们想要通过插件操作页面上的DOM节点，最早可以在和这个阶段中进行

​	beforeUpdate： 页面显示旧数据，保存data中是新数据，只是页面未被渲染。

​	updated：页面显示的数据和data中的数据已经保持同步了，都是最新的

​	beforeDestory：vue实例的销毁阶段，这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于可用状态。

​	destroyed： 完全被销毁。

#### created和mounted的区别

答：created:在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图。

​		mounted:在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作。

#### vue获取数据在哪个周期函数？

答：一般 created/beforeMount/mounted 皆可。比如如果你要操作 DOM , 那肯定 mounted 时候才能操作.



### 请详细说下你对vue生命周期的理解？

答：总共分为8个阶段创建前/后，载入前/后，更新前/后，销毁前/后。

创建前/后： 在beforeCreated阶段，vue实例的挂载元素$el和**数据对象**data都为undefined，还未初始化。在created阶段，vue实例的数据对象data有了，$el还没有。

载入前/后：在beforeMount阶段，vue实例的$el和data都初始化了，但还是挂载之前为虚拟的dom节点，data.message还未替换。在mounted阶段，vue实例挂载完成，data.message成功渲染。

更新前/后：当data变化时，会触发beforeUpdate和updated方法。

销毁前/后：在执行destroy方法后，对data的改变不会再触发周期函数，说明此时vue实例已经解除了事件监听以及和dom的绑定，但是dom结构依然存在。




## Vm和Vc的关系

![](C:\Users\Administrator\Desktop\R星\assets\image-6594189494789498.png)

## ES6 -箭头函数用法和注意点!

1. 箭头函数会改变函数内 `this` 的指向与上级作用域中的this指向保持一致。
2. 不可用当做构造函数，也就是说不能用`new`调用。
3. 不能在里边使用 `arguments` 对象，该对象在箭头函数中不存在，但还是可用使用剩余参数代替。（[剩余参数](https://blog.csdn.net/qq_45466204/article/details/115246614)）

```javascript
es6中因为箭头函数内外 this 指向相同，所以用箭头函数解决。
var student = {
    sname : "LiMing",
    sage : 18,
    friends:["Jenny","Danny"],
    speak:function(){
        console.log(`${ this.sname} is ${this.sage} years old !`);
        this.friends.forEach((elem) => {
                            console.log(`${this.sname} and ${elem} are good friends ！`)
                            })
    }
}
student.speak();  
// LiMing is 18 years old !
// LiMing and Jenny are good friends ！
// LiMing and Danny are good friends ！
错误案例
var student = {
    sname : "LiMing",
    sage : 18,
    friends:["Jenny","Danny"],
    speak:function(){
        console.log(`${ this.sname} is ${this.sage} years old !`);
        this.friends.forEach(function(elem){
                            console.log(`${this.sname} and ${elem} are good friends ！`)
                            })
    }
}
student.speak();  
// LiMing is 18 years old !
// undefined and Jenny are good friends ！
// undefined and Danny are good friends ！

```

## 安装Vue-CLI

详细配置见：[2023最新最全 Vue 安装与配置入门教程(非常详细)，从零基础入门到精通，看完这一篇就够了！！！_vue安装-CSDN博客](https://blog.csdn.net/logic1001/article/details/134617666)

#### 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上

vue --version

#### 安装或者升级你的@vue/cli

npm install -g @vue/cli

#### 创建

vue create vue_test
#### 启动
cd vue_test
npm run serve

版本检测：

```
			node -v 检验nodejs版本

 			vue  -V 检验vue-cli版本安装

			npm list vue 查看vue  版本
```

```
安装vue -cli的步骤：
		淘宝镜像：npm install -g cnpm --registry=https://registry.npm.taobao.org
		npm install -g @vue/cli
		vue create xxxx
		npm run serve
```

## vue特殊属性

### ref属性

1. 作用：用于给节点打标识，用在html的标签上获取真实dom，用在组件上获取组件实例化对象vc 

   ```js
   <h1 ref = "xxxxx"> </h1> 
   <APP ref = "xxx"></APP>
   获取: this.$refs.xxxxxx
   ```

2. 用来给子组件或元素注册引用信息（id的代替着）

### props配置

功能:让组件接收外部传过来的数据

```
(1).传递数据:
	<Demo name="xxx"/>
(2).接收数据:
	第一种方式(只接收):
	props:['name']
第二种方式(限制类型):
	props:{
		name:{
		default:10//默认值
		type:String,
		requied:true,//类型required:true，
		},
		,,,
	}
```

备注:props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中

### mixin混入

一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。

同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。

值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

```js
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})
```

### 插件

```js
// 调用 `MyPlugin.install(Vue)`
Vue.use(MyPlugin)

new Vue({
  // ...组件选项
})
```

## TodoList案例

1.组件化编码流程
		(1).拆分静态组件:组件要按照功能点拆分，命名不要与html元素冲突，
		(2).实现动态组件:考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用:
				1).一个组件在用:放在组件自身即可。
				2).一些组件在用:放在他们共同的父组件上(状态提升)(3).实现交互:从绑定事件开始

2.props适用于:
		(1).父组件 ==>子组件 通信
		(2).子组件 ==>父组件 通信(要求父先给子一个函数)

3.使用v-model时要切记:v-model绑定的值不能是props传过来的值，因为props是不可以修改的!

4.props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。

## 事件

------



### 自定义绑定事件

```js
<Play v-on:diy_mth="getplayname">       <button @click="sendname">发送名字</button>
methoed:{                                methoed:{      send(){this.$emit('diy_mth',this.who);
	getplayname(name)						             this.$emit('diy_mth',this.who);}
	consel.log(name)}}
```

#### 解绑

this.$off()//全部解绑 this.$off('arg1','arg2')

------



### 全局事件总线

组件间的事件通信，使用于任意组件之间。

#### 安装事件总线对象

在main.js中，创建vm时，在它的原型对象上创建。以便许多vc都可以调用。

```js
new Vue({
beforeCreate () { 
Vue.prototype.$globalEventBus = this// 尽量早的执行挂载全局事件总线对象的操作
},
}).$mount('#root')
```

#### 绑定事件

```
发送方：
  	this.$bus.$emit('hello',this.who)
接收方：
	 mounted(){  
         this.$bus.$on('hello',
      	(data)=>{console.log('组件值：',data)}  
    )},
```

#### 分发事件

this.$globalEventBus.$emit('deleteTodo', this.index)

#### 解绑事件

this.$globalEventBus.$off('deleteTodo')

------



### 消息订阅和发送

```
安装 npm i pubsub-js
```

 相关语法
**(1) import PubSub from 'pubsub-js' // 引入**
**(2) PubSub.subscribe(‘msgName’, functon(msgName, data){ })**
**(3) PubSub.publish(‘msgName’, data): 发布消息, 触发订阅的回调函数调用**
**(4) PubSub.unsubscribe(token): 取消消息的订**

##  解决Ajax 跨域问题

配置代理服务器



## Vuex

专门在 Vue 中实现集中式状态**（数据）**管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。

####    actions

1. 值为一个对象，包含多个响应用户动作的回调函数

2. 通过 commit( )来触发 mutation 中函数的调用, 间接更新 state

3. 如何触发 actions 中的回调？
    在组件中使用: $store.dispatch('对应的 action 回调名') 触发

4. 可以包含异步代码（定时器, ajax 等等）


####   mutations
1. 值是一个对象，包含多个直接更新 state 的方法

2. 谁能调用 mutations 中的方法？如何调用？
    在 action 中使用：commit('对应的 mutations 方法名') 触发

3. mutations 中方法的特点：不能写异步代码、只能单纯的操作 state  


####   geters

   概念:当state中的数据需要经过加工后再使用时，可以使用getters加工:

```js
在 index.js 中追加 getters 配置
		const getters =
			bigSum(state){
				return state.sum*10}
				
		//创建并暴露
		store export default new Vuex.Store({
		getters)}
		
组件中读取数据:$store.getters.bigSum
```

####    modules

8. 包含多个 module

9. 一个 module 是一个 store 的配置对象

10. 与一个组件（包含有共享数据）对应

### 工作原理

“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)。action是可以用来处理任何异步请求的。对于数据的修改还需要通过commit之后的mutation来进行 修改。也可以通过state直接调用mutation进行数据的修改。

![](C:\Users\Administrator\Desktop\R星\assets\vuex.png)

### 使用：

1.  导入vuex :  vue2用vuex@3导入，vue3导入vuex@4

```
   npm i vuex@3 //不同版本修改不同的参数
```

2.  编写文件：

​      store管理Action、mutation、state三个对象.

```js
    index.js
	1.  引入Vue ,引用Vuex.
	2.  Vue（Vuex）
	3.  通过Vue.Store()new三个对象
```

3. 在mian.js中导入文件，并使用$store来操作数据。

### 操作：

```
$store.commit()   //直接提交给mutatuion进行运算

$store.dispatch()  //直接提交给action进行处理



action中传入两个参数（context,value）

mutation中传入两个参数（state,value）

state中保存计算值的初始化
```

### 四个Map方法的使用

#### mapstate方法

```js
  computed:{
  		//借助mapstate生成计算属性:sum、school、subject(对象写法)}
 		...mapState({sum:'sum',school:'school',subject:'subject'})

       //借助mapstate生成计算属性:sum、school、subiect(数组写法)
        ...mapState(['sum','school','subject'])}
```
#### mapGetters方法

​		 用于帮助我们映射 getters 中的数据为计算属性

```
//借助mapGetters生成计算属性:bigsum(对象写法)	...mapGetters({bigSum:'bigSum'})
//借助mapGetters生成计算属性:bigsum(数组写法)	...mapGetters(['bigSum'])
```

#### mapActions方法

​		用于帮助我们生成与 actions对话的方法，即:包含$store.dispatch(xxx)的函数

```
//靠mapActions生成:incrementOdd、incrementWait(对象形式)式)   												   
					      					   ...mapActions({incrementOdd:'jiadd',incrementWait:'jiaWait'})
//靠mapActions生成:incrementOdd、incrementWait(数组形式)                    ...mapActions(['jia0dd','jiawait'])
```

#### mapMutation方法

```
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
```



## 路由

路由就是SPA（单页应用）的路径管理器。再通俗的说，vue-router就是我们WebApp的链接路径管理系统。

用来管理单页面的跳转。

------

### 使用

安装vuerouter。编写对应的router文件。使用Vue.use(VueRouter)

vuerouter解析a标签，实现切换(active-class可配置高亮样式)

```js
<router-link active-class="active'"to="/about">About</router-link>
```

指定展示位置               <router-view></router-view>

### 注意点

1.路由组件通常存放在 pages 文件夹，一般组件通常存放在components 文件夹。

2.通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载

3.每个组件都有自己的 $route 属性，里面存储着自己的路由信息

4.整个应用只有一个router，可以通过组件的 $router 属性获取到,

### 嵌套路由

1.配置路由规则，使用children配置项:

```js
routes:[
{
	path:'/about',          //一级路由需要写路径
	component:About,
		children:[           //通过children配置子级路由
		{
		 path:'news',        //此处一定不要写,会自动给子路由添加/
		 component:News,
		}]
}]
```

2.跳转(要写完整路径):<router-link to="/home/news">News</router-link>

### 路由query传参

​	1.**传递参数**

```js
<!--跳转并携带query参数，to的字符串写法-->
	<router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>
<!--跳转并携带query参数，to的对象写法 -->
	 <router-link   :to="{
		path:'/home/message/detail'
		query:{
				id:666，
				title:'你好
> 跳转</router-link>
```

**2.接收参数** :                $route.query.id             $route.query.title



## 组件库

常见的ui组件库

```
移动端的组件库
1.Vanthttps://youzan.github.io/vant
2. Cube Ulhttps://didi.github.io/cube-ui<
3. Mint Ulhttp://mint-ui.github.io

PC 端常用 
1.Element Ul https://element.eleme.cn
2. lView Ulhttps://www.iviewui.com
```

element ui ：

全部引入和按需引入

# 注意

------

### 组件data是函数而不是对象

答：实例的时候定义`data`属性既可以是一个对象，也可以是一个函数。

通过组件的构造函数定义data属性的时候，在实例化不同组件abc的过程中，两个组件会公用一个内从地址，会产生数据污染。

```js
function Component(){
 
}
<br>
Component.prototype.data = {
	count : 0
}
<br>
const componentA = new Component()
const componentB = new Component()
<br>
console.log(componentB.data.count)  // 0
componentA.data.count = 1
console.log(componentB.data.count)  // 1
```

采用函数的形式，则不会出现这种情况（函数返回的对象内存地址并不相同）

```js
function Component(){
	this.data = this.data()
}
Component.prototype.data = function (){
    return {
   		count : 0
    }
}
```

vue esm =es module

vue=核心配置文件+模版解析器 ，残缺版的vue没有模版解析器，极大的缩小了体积。webpack打包过程中会将模版解析器打包增大文件体积。

运行时构建不包含模板编译器，因此不支持template选项，只能用render选项。但即使使用运行时构建，在单文件组件中也依然可以写模板，因为单文件组件的模板会在构建时预编译为render函数。

模板解析器是把HTML中的模板解析成json对象AST，这个对象包含了dom的各种属性，在非单文件中必须要模板解析器。但是单文件中的组件都是编译之后的，组件中直接就是ast，就不需要模板解析器了。

### ES 模块化

[ECMAScript](https://so.csdn.net/so/search?q=ECMAScript&spm=1001.2101.3001.7020)模块（简称ES模块）是一种JavaScript代码重用的机制，于2015年推出，一经推出就受到前端开发者的喜爱。在2015之年，JavaScript 还没有一个代码重用的标准机制。多年来，人们对这方面的规范进行了很多尝试，导致现在有多种模块化的方式。

ES 6.0，就是JS下一代的语法标准，6.0版本的JS可以这样理解，在2015年6月正式发布的6.0，而现在许多JS框架、项目等，还在遵循ES5标准。其中let、const、箭头函数 、模版字符串、数组的拓展常用。



vue在打包的过程中webpack会使用nodejs运行时环境，node.js使用commonjs模块化规范，es6的模块化和commonjs的模块化对比，从而进行更改配置。

commonjs模块和es6模块最主要的区别：
    1.commonjs模块是拷贝（能修改其值），es6模块是引用（只存在只读状态，不能修改其变量值）
    2.CommonJS模块是运行时加载，ES6模块是编译时输出接口

![](C:\Users\Administrator\Desktop\R星\assets\image-20240227210918067.png)

### Vue启动问题

#### vue项目时提示“Failed to check for updates”

通过一番搜索查找之后，发现出现这个提示是因为淘宝源证书过期的问题

1. 重新安装了一下淘宝镜像

   ```
   npm config set registry http://registry.npm.taobao.org/
   //官方淘宝镜像，上面下面安装其中一个即可
   npm config set registry https://registry.npmjs.org/
   ```

2. 关闭npm的证书检查

   ```
   npm cache clean --force
    
   npm config set strict-ssl false
    
   npm install
   ```

### this指向原则：

   ​	1.所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象

   ​	2.所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，这样this的指向才是vm 或 组件实例对象

### ...mapState理解

   扩展运算符(...)将`mapState`返回的对象合并到当前组件的计算属性中

   **扩展运算符**：（spread运算符）用三个点号（…）表示，功能是把数组或类数组对象**展开**成一系列用逗号隔开的值。

   ```js
   let foo = function(a, b, c) {
       console.log(a);
       console.log(b);
       console.log(c);}           let arr = [1, 2, 3];     foo(...arr);//1//2//3
   ```

   **剩余运算符（rest）**：也是三个点号，将逗号隔开的参数**合成**一个数组。

   ```js
   //主要用于不定参数，所以ES6开始可以不再使用arguments对象
   bar = function(a, ...args) {
       console.log(a);
       console.log(args);
   }
   
   bar(1, 2, 3, 4);
   //1
   //[ 2, 3, 4 ]
   
   ```

   **解构赋值**：`（对于数组、对象、字符串都可以）`

   ```js
   数组：
   let arr = ['this is a string', 2, 3];
   let [a, b, c] = arr
   console.log(a);//this is a string
   console.log(b);//2
   console.log(c);//3
   对象：
   let { foo, bar } = { foo: 'aaa', bar: 'bbb' }
   foo // "aaa"
   bar // "bbb"
   字符串：
   const [a, b, c, d, e] = 'hello';     a // "h"   b // "e"   c // "l"  d // "l"  e // "o"
   ```

尚硅谷

# vue启动问题

### 1.

### [10% building 2/5 modules 3 active ...lib\index.js!/路径报错](https://www.cnblogs.com/1314520zqj/p/17408849.html)

```ruby
临时解决方案  终端输入 
$env:NODE_OPTIONS="--openssl-legacy-provider"

或者启动项目时 更改package.json中的配置
```

"dev": "SET NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",



2.

#### vue项目启动时，报错：TypeError: Cannot read property ‘range’ of null

babel-lint版本过高问题导致。
可以在项目根目录下查看package.json文件，查看babel-eslint的版本：



1.在终端执行npm install babel-eslint@7.2.3 ，安装babel-eslint稳定版；

2、关闭项目，在本地删除node_modules文件夹，重新在编辑器打开项目；





### 3.

### npm设置镜像的方法

npm config get registry



淘宝：npm config set registry  https://registry.npmmirror.com


原像：`npm config set registry https://registry.npmjs.org`



### 4.

// 终端报错如下

npx update-browserslist-db@latest 
Why you should do it regularly: https://github.com/browserslist/update-db#readme

它是配合 autoprefixer 来给 CSS 做兼容处理的，你可以理解为 CSS 里的 babel；Vue 里的 vue-loader 会在 PostCSS 这步调用它。升级版本。

 npm install autoprefixer@latest caniuse-lite@latest browserslist@latest --save-dev



# pnpm和corepack

node.jsv16以后使用的时corepack打包方式。但是在安装nodejs的过程中不会安装corepack，需要自己手动安装。使用pnpm是，版本需要在node.js16以上。

pnpm的优势：下载nodemodule模块时会记录下载的存储访问地址，以便更新的操作重复读取，减少了内存的开销，加快了模块的相应速度。

