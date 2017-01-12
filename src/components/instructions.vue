<template>
	<div id="instructions">
		<h3 class="title">{{title}}</h3>
		<p class="text">{{first}}</p>
		<p class="text">{{second}}</p>
		<p class="text">{{third}}</p>
	</div>
</template>

<script>
	export default {
		data:function(){
			return {
				title:"The Intructions of Todolists:",
				first:"1.输入框输入的时候必须要有内容，否则会有提示",
				second:"2.每条信息都有删除键，可以进行删除",
				third:"3.当事项加入列表后可以点击，会出现'finished'字段，并且颜色变浅加下划线"
			}
		}
	}
</script>

<style>
#instructions{
	width:260px;
	border:1px solid black;
	border-left:none;
	border-radius:8px;
	padding:20px 5px;
}
.title{
	margin:10px 0;
}
.text{
	margin:5px 0px;
	font-size:12px;
}
p:first-letter{
	font-size:16px;
	font-weight:bold;
}

</style>