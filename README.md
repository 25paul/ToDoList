# ToDoList
> this is a todolist
## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 项目简介
这个项目时用vue.js实现的，并且使用了vue.js的脚手架工具vue-cli+webpack搭建的，可以将.vue文件（通过webpack）打包成浏览器能够识别的文件。<br/>

主要是实现一个todolist，如下图：<br/>
![image](https://github.com/25paul/ToDoList/tree/master/images/first.png)<br/>
![image](https://github.com/25paul/ToDoList/tree/master/images/second.png)

## 功能概要：
Input输入框必须要有内容输入才能按enter键，不然获得焦点后按enter键的话会提示输入内容；
* 添加列表后还有删除按钮，随时都可以进行删除操作；
* 当列表中的事项完成之后，可以点击，会出现一个“finished”的标志，以及下划线；
* 列表中的事项也都保存咋本地的localStorage，不会丢失。

## 开发环境的搭建
详情可以[查看官网](https://github.com/vuejs/vue-cli)<br/>
[中文介绍](http://www.tuicool.com/articles/veIRziU)<br/>
这个开发环境可以帮忙下载一套基于vue.js开发的一个模板，包括vue.js开发的一个基础框架、vue.js打包的工具、测试工具、开发调试的环境（服务器等）。<br/>
我的npm是用淘宝的镜像：[详情](http://www.cnblogs.com/Chen-XiaoJun/p/6236625.html)<br/>
所生成的文件列表中的Index.html和main.js是入口文件，相当于平时开发中主程序代码引入js文件。即index.html会默认调用main.js

## 项目要点：
涉及到的知识点：前端开发基础：HTML,CSS,JS；前端模块化基础；ES6基础。<br/>

### import：
import是ES6的语法<br/>
相当于：var App = require(‘...’)。

### export：
在vue脚手架里面，export出来的东西，会自动生成new Vue({...})。

### 关于data:
[组件中的data必须是函数](https://cn.vuejs.org/v2/guide/components.html#data-必须是函数)<br/>
在ES6中，data :function(){...}也可以这样写：data () {...}

### 输入框input：
绑定一个keyup.enter事件，当按下enter键时触发addNew方法：<br/>
```
addNew:function(){
      if(!this.$refs.input.value){
      	alert("please input some content!"); 
      	return 0
      }
      this.items.push({
        label:this.newItem,
        "isFinished":false
      })
      this.newItem=""
},
```
这里还通过ref来引用DOM元素，使用引用信息this.$refs.input.value来获取input输入的值，当值为空（即没有输入的时候），按下enter键会提示：“please input some content!”；当输入值不为空的时候，执行后面代码：将与input双向绑定的数据newItem赋给label，isFinished值为“false”，并且添加到数组里面；最后还要清空input输入框的内容：this.newItem=""。<br/>

### 这里还要给列表li绑定class类和点击事件：<br/>
```
<li v-bind:class="{finished:item.isFinished}" v-on:click="toggleFinish(item)">
toggleFinish:function(item){
  item.isFinished = !item.isFinished
},
```
用来切换列表点击时的效果：完成与未完成。<br/>
样式表如下：<br/>
```
.finished span{
  	text-decoration:underline;
  	color:#EEEEE0;
}
.finished span:after{
	content:"finished";
	width:45px;
	height:15px;
	border-radius:2px;
	font-size:12px;
	text-align:center;
	line-height:15px;
	position:absolute;
	top:-10%;
	left:-8%;
	display:inline-block;
	background:red;
}
```
### localStorage
使用localStorage来存储列表，建立一个store.js组件：<br/>
```
const STORAGE_KEY='todos-vuejs' 
export default {
	fetch:function(){
		return JSON.parse(window.localStorage.getItem(STORAGE_KEY)||'[]');
	},
	save:function(items){
		window.localStorage.setItem(STORAGE_KEY,JSON.stringify(items))
	}
}
```
为了让数据存储能够得到复用，我们这里用到watch来观察数据的变化。这时候添加items或者点击修改items的值都会触发这个watch。<br/>
这里的deep:true的作用就是：允许item里面的key值被修改。比如在这个示例中，当我们添加了item之后，在点击的时候还要修改item的状态值：isFinished，这是后如果deep设置为false就无法触发watch了。</br>
```
watch:{
items:{
  handler:function(items){
    Store.save(items)
  },
  deep:true
}
},
```
当items变化时就保存在localStorage里面<br/>
这是就可以在data里获取来渲染在页面上：<br/>
```
items:Store.fetch()
```
### 删除按钮：
```
deleteClick:function(index){
	this.items.splice(index, 1);
}
```
```
<p class="item-delete" @click.stop="deleteClick(index)">Delete</p>
```
给DOM元素添加一个点击事件<br/>
这里有一个小小的问题：就是因为Delete是包裹在li里面的，所以要给点击事件添加修饰符.stop用来组织事件冒泡，不然点击Delete之后还会触发li。
