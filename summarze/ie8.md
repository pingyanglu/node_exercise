### ie8的兼容性问题
---
1. 绝对定位fixed<br>

`

    .ie{  
        _position: absolute; 
        _clear: both;  
        _top:expression(eval(document.compatMode &&  
                document.compatMode=='CSS1Compat') ?  
                documentElement.scrollTop  
                +(documentElement.clientHeight-this.clientHeight) - 1  
                : document.body.scrollTop  
                +(document.body.clientHeight-this.clientHeight) - 1);
    }

`
---
2. 透明度问题<br>
`

    .mask{
        background:black !important;
        filter:alpha(opacity=90);
    }
`
---
3. 图片的load方法不执行 
`

    $("<img/>").attr("src", src).load(function(){})不执行
    改成
    $("<img/>").attr("src",src).one('load',function(){}).each(function() {if(this.complete){$(this).load()};});  
`
---    

4. JSON.parse()与JSON.stringify()不执行，
    可以引入json2.js来解决
---
5. foreach方法不兼容，使用for循环
---
6. create方法不兼容

`

    if (typeof Object.create !== "function") {
			Object.create = (function() {
				function Temp() {}
				var hasOwn = Object.prototype.hasOwnProperty;
				return function (O) {
				  if (typeof O != 'object') {
					throw TypeError('Object prototype may only be an Object or null');
				  }
				  Temp.prototype = O;
				  var obj = new Temp();
				  Temp.prototype = null; 
				  if (arguments.length > 1) {
					var Properties = Object(arguments[1]);
					for (var prop in Properties) {
					  if (hasOwn.call(Properties, prop)) {
						obj[prop] = Properties[prop];
					  }
					}
				  }
				  return obj;
				};
			  })();
		}


`
------

7. text-align:center不支持.使用margin:0 auto;来实现居中。

----

8. 伪类伪元素不支持，一些使用伪类伪元素制作的小图标不能显示。

---

9. ie8兼容hack代码:

`

	<!--[if !IE]> -->
	<!-- <![endif]-->

	<!--[if IE]>
	<![endif]-->

	<!--[if lte IE 8]>
	<![endif]-->

	

`