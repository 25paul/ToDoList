<template>
  <div id="app">
  	<div class="header">
	    <h2 class="title" v-html = "title"></h2>
	    <input ref="input" placeholder="添加ToDo" required="required" v-model="newItem" v-on:keyup.enter="addNew" ></input>
    </div>
    <ul class="list">
      <li v-for="(item,index) in items" v-bind:class="{finished:item.isFinished}" v-on:click="toggleFinish(item)">
      	<span>{{item.label}}</span>
      	<p class="item-delete" @click.stop="deleteClick(index)">Delete</p>
      </li>
    </ul>
    <Instructions class="instructions"></Instructions>
  </div>
</template>

<script>
import Store from './store'
import Instructions from './components/instructions'

console.log(Date())

export default {
  data:function(){
    return {
      title:"This Is A ToDoList",
      items:Store.fetch(),
      newItem:""
    }
  },
  watch:{
    items:{
      handler:function(items){
        Store.save(items)
      },
      deep:true
    }
  },
  methods:{
    toggleFinish:function(item){
      item.isFinished = !item.isFinished
    },
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
    deleteClick:function(index){
    	this.items.splice(index, 1);
    }
  },
  components: {
  	Instructions
  }
}
</script>

<style>
*{
  	margin:0;
  	padding:0
}
body{
	background:#FFFFF0
}
#app {
	width:100%;
  	font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    position:relative;
}
.list{
	width:51%;
	margin:20px auto;
}
.list .item-delete{
	display:inline-block;
	width:50px;
	height:30px;
	background:#8B8378;
	border-radius:8px;
	color:#FFFFFF;
	cursor:pointer;
	text-align:center;
	line-height:30px;
	font-size:12px;
	position:absolute;
	right:0;
	index:10;

}
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
li{
  list-style:none;
  font-size:1.6em;
  margin-top:20px;
  position:relative;
}
.header{
	width:100%;
	height:50px;
	text-align: center;
	background:rgba(0, 0, 0, 0.8);
}
.header .title{
	display:inline-block;
	margin:11px 150px 11px 0;
	color:#fff
}
.header input{
	width:25%;
	height:24px;
	diaplay:inline-block;
	margin:-12px;
	text-indent:10px;
	border-radius:5px;
	box-shadow: 0 1px 0 rgba(255,255,255,0.24), 0 1px 6px rgba(0,0,0,0.45) inset;
	border:none
}
.instructions{
	position:fixed;
	bottom:0;
}

</style>
