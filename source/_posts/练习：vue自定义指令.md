---
title: 练习：vue自定义指令
date: 2018-05-02 15:57:07
categories:
  - vue
tags:
  - vue
---

这是很少会更新的附带完整代码的博客。
究其原因，有些并不复杂，但不方便总结的知识点，直接看源码更方便。
<!--more-->
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>练习：自定义指令</title>
	<script src="js/vue.js"></script>
	<style>
		#itany div{
			width: 100px;
			height: 100px;
			position:absolute;
		}
		#itany .hello{
			background-color:red;
			top:0;
			left:0;
		}
		#itany .world{
			background-color:blue;
			top:0;
			right:0;
		}

	</style>
</head>
<body>
	<div id="itany">
		<div class="hello" v-drag>itany</div>
		<div class="world" v-drag>网博</div>
	</div>

	<script>
        // vue全局自定义指令
		Vue.directive('drag',function(el){
			el.onmousedown=function(e){
				// 获取鼠标点击处分别与div左边和上边的距离：鼠标位置-div位置
				var disX=e.clientX-el.offsetLeft;
				var disY=e.clientY-el.offsetTop;
				// console.log(disX,disY);

				//包含在onmousedown里面，表示点击后才移动，为防止鼠标移出div，使用document.onmousemove
				document.onmousemove=function(e){
					// 获取移动后div的位置：鼠标位置-disX/disY
					// 因为offset获取的是左边和上边，所以移动需使用减法
					var l=e.clientX-disX;
					var t=e.clientY-disY;
					el.style.left=l+'px';// 记得要加px像素单位,否则不移动
					el.style.top=t+'px';
				}

				//停止移动，鼠标抬起触发
				document.onmouseup=function(e){
                    // 清空鼠标按住状态
					document.onmousemove=null;
                    // 销毁自身
					document.onmouseup=null;
				}

			}
		});

		var vm=new Vue({
			el:'#itany',
			data:{
				msg:'welcome to itany',
				username:'alice'
			},
			methods:{
				change(){
					this.msg='欢迎来到南京网博'
				}
			}
		});
	</script>
	
</body>
</html>
```