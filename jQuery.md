# jQuery

## 是什么

jQuery 是一个**快速，小巧，功能丰富的 JavaScript 库**。 它通过易于使用的 API 在大量浏览器中运行，使得 HTML 文档遍历和操作，事件处理，动画和 Ajax 变得更加简单。 通过多功能性和可扩展性的结合，jQuery 改变了数百万人编写 JavaScript 的方式。



## 使用原因

- 写少代码,做多事情【**write less do more**】

- 免费，开源且轻量级的 js 库，容量很小 

- **兼容市面上主流浏览器**，例如 IE，Firefox，Chrome 

- **能够处理 HTML/JSP/XML、CSS、DOM、事件、实现动画效果，也能提供异步 AJAX 功能** 

- **文档手册很全，很详细** 

- **成熟的插件可供选择**，多种 js 组件，例如日历组件（点击按钮显示下来日期） 

- 出错后，有一定的提示信息 

- 不用再在 html 里面通过\<script>标签插入一大堆 js 来调用命令了





## DOM\JavaScript\JQuery对象

- **DOM：文档对象模型（Document Object Model，简称 DOM），是 W3C 组织推荐的处理可扩展 标志语言的标准编程接口。**

- **通过 DOM 对 HTML 页面的解析，可以将页面元素解析为元素节点、属性节点和文本节点，这些解析出的节点对象，即 DOM 对象。DOM 对象可以使用 JavaScript 中的方法。**



- **JavaScript：用 JavaScript 语法创建的对象叫做 JavaScript 对象, JavaScript 对象只能调用 JavaScript 对象的 API。**
- **JQuery：用 JQuery 语法创建的对象叫做 JQuery 对象, jQuery 对象只能调用 jQuery 对象的 API。 jQuery 对象是一个数组。在数组中存放本次定位的 DOM 对象**
- Jquery对象是一个DOM对象的数组，可以使用get(下标)或者直接加[下标]（下标从0开始）获取DOM对象

### DOM对象和JQuery对象转换

**需求：对应对象才能调用对应的API的函数，所有有时会进行转换**

**DOM --> jQuery：调用\$(DOM对象)将参数的DOM对象转为jQuery对象**

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Document</title>
		<script src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			function btnClick() {
				var obj = document.getElementById("btn");
				alert("使用dom对象的属性" + obj.value);

				var $btn = $(obj);
				alert($btn.val());
			}
		</script>
	</head>
	<body>
		<input type="button" id="btn" value="我是按钮" onclick="btnClick();" />
	</body>
</html>

```



**jQuery --> DOM：调用\$(DOM对象)将参数的DOM对象转为jQuery对象**

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Document</title>
		<script src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			function btnClick() {
				// 要用value属性，就必须转为DOM对象
				var txt = $("#txt")[0];
				var value = txt.value;
				txt.value = value * value;
			}
		</script>
	</head>
	<body>
		<input type="button" id="btn" value="计算平方" onclick="btnClick();" /><br>
		<input type="text" id="txt" value="整数" /><br>
	</body>
</html>
```





## 选择器

**常用：**

- **id选择器：\$("#id") 获取指定id的DOM元素，id是唯一的，多个元素使用同一个标签时，只有第一个元素会生效**
- **class选择器：\$(".class") 获取指定样式class的DOM元素，多个元素可以设置同一个class**
- **标签选择器：$("标签名") 获取指定标签的所有DOM元素**



不常用：

- 所有选择器：$("\*") 获取页面所有DOM元素
- 组合选择器：使用逗号分隔不同选择器语法，选择对应符合的DOM对象

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Document</title>
		<style type="text/css">
			div {
				background: gray;
				width: 200px;
				height: 100px;
			}
		</style>
		<script src="js/jquery-3.6.0.js"></script>
		<script type="text/javascript">
			function fun1() {
				// id选择器
				var obj = $("#one");
				alert(obj.length);
				// 使用Jquery之中改变样式的函数
				obj.css("background", "red");
			}

			function fun2() {
				// class选择器
				var obj = $(".two");
				// 使用Jquery之中改变样式的函数
				obj.css("background", "yellow");
			}

			function fun3() {
				// 标签选择器，里面有三个成员，全部设置
				var obj = $("div");
				// 使用Jquery之中改变样式的函数
				obj.css("background", "green");
			}

			function fun4() {
				var obj = $("*");
				obj.css("background", "blue");
			}

			function fun5() {
				var obj = $("#one,span");
				obj.css("background", "blue");
			}
		</script>
	</head>
	<body>
		<div id="one">我的id=one的div</div><br>
		<div class="two">我没有id,class=two的div</div><br>
		<div>我是没有id,class的div</div><br>
		<span class="two">我是span标签</span><br>
		<input type="button" value="获取id是one的dom对象" onclick="fun1();" />
		<input type="button" value="获取class是two的dom对象" onclick="fun2();" />
		<input type="button" value="获取div标签的dom对象" onclick="fun3();" />
		<input type="button" value="获取id为one和span标签dom对象" onclick="fun5();" />
	</body>
</html>

```







