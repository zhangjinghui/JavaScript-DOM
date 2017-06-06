# JavaScript DOM 笔记  

---

1. 结点（Node）的类型：  
 + 元素结点  
 + 属性结点  
 + 文本结点

2. js代码建议放在 `<title/>` 标签之后，例如：  
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	//1. 当整个 HTML 文档完全加载成功后触发 window.onload 事件. 
	//事件触发时, 执行后面 function 里边的函数体. 
	window.onload = function(){
		//2. 获取所有的 button 节点. 并取得第一个元素
		var btn = document.getElementsByTagName("button")[0];
		//3. 为 button 的 onclick 事件赋值: 当点击 button 时, 执行函数体
		btn.onclick = function(){
			//4. 弹出 helloworld
			alert("helloworld");
		}
	}	
</script>
</head>
<body>
	
	<button>ClickMe</button>
	
</body>
</html>
```
3. 如何获取 **元素结点** ?  
 + document.getElementById(""); 根据 id 属性获取单个结点  
 + document.getElementsByTagName(""); 根据标签名获取结点数组，数组对象的 length 属性可以获取数组的长度 
 + document.getElementsByName(""); 根据 name 属性获取结点数组（若某节点 `<li/>` 没有 name 属性，而是自定义的 name 属性，IE 浏览器不支持，但其他内核浏览器支持，使用时需谨慎）  

4. 读写 **属性结点**   
> 最佳的方式：通过 **元素结点.属性名** 的方式来获取属性值和设置属性值  

5. 获取 **元素结点** 的子节点（只有元素结点才有子节点）  
 + ：childNode 属性，获取全部子节点，但该方法不实用，用 getElementsByTagName(""); 方法替代  
 + firstChild 属性，获取第一个子节点  
 + lastChild 属性，获取最后一个子节点  

6. 读写 **文本结点**  
 + 获取元素结点的子节点  
 + 若元素结点只有一个文本结点，例如： `<li id="bj">北京</li>` ，可以先获取元素结点 liNode，再通过 liNode.firstChild.nodeValue 读写文本结点的值  

7. **function** 的使用  
```javascript
function test(){
	alert("test...");
}
// 将函数返回的结果赋值给事件
btn.onclick = test();
``` 
```javascript
// 为事件确定所要调用的函数的引用
function test(){
	alert("test...");
}
btn.onclick = test;
``` 
```javascript
// 使用匿名函数实现简写
btn.onclick = function(){
	alert("test...");
}
```
8. for循环结点列表时，**this** 为正在响应事件的那个结点(事件被一个函数的引用赋值， **this** 就是调用函数的那个对象)  
```javascript
<script type="text/javascript">
	window.onload = function(){			
		//点击每个 li 节点, 都弹出其文本值. 	
		//1. 获取所有的 li 节点
		var liNodes = document.getElementsByTagName("li");		
		//2. 实用 for 循环进行遍历. 得到每一个 li 节点
		for(var i = 0; i < liNodes.length; i++){
		//3 为每一个 li 节点添加 onclick 响应函数. 
			liNodes[i].onclick = function(){
				//4. 在响应函数中获取当前节点的文本节点的文本值
				//5. alert 打印. 
				//this 为正在响应事件的那个节点. 
				alert(this.firstChild.nodeValue);			
				//此时 i 已经是 8 了, 而 liNodes[8] 不指向任何节点.
				//alert(i);
		}			
	}						
}
</script>
```
9. JavaScript 中的 **正则表达式**  
 + 双斜杠：`var re=/^\?(\w{1,}=\w{1,}&){1,}\w{1,}=\w{1,}/;`  
 + 构造函数：`var re=new RegExp(/^\?(\w{1,}=\w{1,}&){1,}\w{1,}=\w{1,}/);`  
 + boolean rexp.test(String ss);  
 + 去除收尾空格 `/^\s*|\s*$/g`

10. nodeType、nodeName、nodeValue  
```javascript
//1. 元素节点的这 3 个属性
var bjNode = document.getElementById("bj");
alert(bjNode.nodeType); //元素节点: 1
alert(bjNode.nodeName); //节点名: li
alert(bjNode.nodeValue); //元素节点没有 nodeValue 属性值: null

//2. 属性节点
var nameAttr = document.getElementById("name")
                       .getAttributeNode("name");
alert(nameAttr.nodeType); //属性节点: 2
alert(nameAttr.nodeName); //属性节点的节点名: 属性名
alert(nameAttr.nodeValue); //属性节点的 nodeValue 属性值: 属性值

//3. 文本节点:
var textNode = bjNode.firstChild;	
alert(textNode.nodeType); //文本节点: 0
alert(textNode.nodeName); //节点名: #text
alert(textNode.nodeValue); //文本节点的 nodeValue 属性值: 文本值本身. 

//nodeType、nodeName 是只读的.
//而 nodeValue 是可以被改变的。 
//以上三个属性, 只有在文本节点中使用 nodeValue 读写文本值时使用最多. 
```
11. 创建结点  
 + document.**createElement**(""); 创建一个元素节点  
 + document.**createTextNode**(""); 创建一个文本结点  
 + elementNode.**appendChild**(newChild); 将子节点添加到父节点列表的最后  

12. 可通过 **`if(元素节点.属性名)`** 判断指定的属性是否存在  
13. 替换节点  
 + var reference = element.**replaceChild**(newChild,oldChild);  父节点调用该方法，将旧结点替换为新节点，返回旧节点的引用。该方法除了替换功能以外还有移动的功能。  
 + 该方法只能单向替换，若需要使用双向替换，需要自定义函数  

```javascript
/**
 * 互换 aNode 和 bNode
 */
function replaceEach(aNode, bNode){
	
	if(aNode == bNode){
		return;
	}
	
	var aParentNode = aNode.parentNode;
	//若 aNode 有父节点
	if(aParentNode){
		var bParentNode = bNode.parentNode;
		
		//若 bNode 有父节点	
		if(bParentNode){
			var tempNode = aNode.cloneNode(true);
			bParentNode.replaceChild(tempNode, bNode);
			aParentNode.replaceChild(bNode, aNode);	
		}
	}
} 
```
14. 删除节点  
 + var reference = element.**removeChild**(node);  父节点调用该方法删除指定的子节点（若该节点包含子节点，也一并删除）。返回被删除节点的引用  
 + 可通过 **parentNode** 属性获取父节点  

15. confirm 确认框  
```javascript
var flag = confirm("确定要删除"+ this.firstChild.nodeValue +"吗?");
```
16. 插入节点  
 + var reference =  element.**insertBefore**(newNode,targetNode);  父节点调用该方法，将新节点插入到目标节点之前，返回被替换节点的引用。该方法除了插入功能以外还有移动的功能。（w3c标准并没有提供 **insertAfter()** ,用户可自定义）  
```javascript
//把 newNode 插入到 refNode 的后面
function insertAfter(newNode, refNode){
	//1. 测试 refNode 是否为其父节点的最后一个子节点
	var parentNode = refNode.parentNode;
	if(parentNode){
		var lastNode = parentNode.lastChild;
		
		//2. 若是: 直接把 newNode 插入为 refNode 父节点的最后一个子节点. 
		if(refNode == lastNode){
			parentNode.appendChild(newNode);
		}
		//3. 若不是: 获取 refNode 的下一个兄弟节点, 然后插入到其下一个兄弟
		//节点的前面.
		else{
			var nextNode = refNode.nextSibling;
			parentNode.insertBefore(newNode, nextNode);
		}
	}
}
```
17. **innerHTML** 属性  
 + 浏览器几乎都支持该属性，但该属性并不包含在 DOM 标准内。  
 + innerHTML 可以用于读取指定标签内的 HTML代码  

18. [JavaScript-DOM](https://github.com/zhangjinghui/JavaScript-DOM)
