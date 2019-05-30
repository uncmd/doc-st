# jQuery语法

## jQuery 选择器

**选择器允许您对元素组或单个元素进行操作**

### 元素选择器

> $("p") 选取 &lt;p&gt; 元素。

> $("p.intro") 选取所有 class="intro" 的 &lt;p&gt; 元素。

> $("p#demo") 选取所有 id="demo" 的 &lt;p&gt; 元素。

### 属性选择器

> $("[href]") 选取所有带有 href 属性的元素。

> $("[href='#']") 选取所有带有 href 值等于 "#" 的元素。

> $("[href!='#']") 选取所有带有 href 值不等于 "#" 的元素。

> $("[href$='.jpg']") 选取所有 href 值以 ".jpg" 结尾的元素。

### CSS选择器

> $("p").css("background-color","red");

## jQuery 事件

| Event 函数          | 绑定函数至 |
| ------------- | ------------- |
| $(document).ready(function) | 将函数绑定到文档的就绪事件（当文档完成加载时）|
| $(selector).click(function) | 触发或将函数绑定到被选元素的点击事件 |
| $(selector).dblclick(function) | 触发或将函数绑定到被选元素的双击事件 |
| $(selector).focus(function) | 触发或将函数绑定到被选元素的获得焦点事件 |
| $(selector).mouseover(function) | 触发或将函数绑定到被选元素的鼠标悬停事件 |

**var jq=jQuery.noConflict()**，帮助您使用自己的名称（比如 jq）来代替 $ 符号。

## jQuery 效果

*speed 参数：可以取值 "slow"、"fast" 或毫秒*

*callback 参数：函数执行完成后所执行的函数名称*

### 滑动

> 向下滑动 slideDown(speed, callback);

> 向上滑动 slideUp(speed, callback);

> 上下切换 slideToggle(speed, callback);

### 淡入淡出

> 淡入 fadeIn(speed, callback);

> 淡出 fadeOut(speed, callback);

> 切换 fadeToggle(speed, callback);

> 渐变为给定的不透明度(0-1) fadeTo(speed, opacity, callback);

### 隐藏显示

> 隐藏 $(selector).hide(speed, callback);

> 显示 $(selector).show(speed, callback);

> 切换 $(selector).toggle(speed, callback);

### 动画

**语法：**

> $(selector).animate({params}, speed, callback);

必须的 params 参数定义形成动画的 CSS 属性

**实例**

```javascript
$("button").click(function(){
    $("div").animate({left:'250px'});
});
```

<font color=#fe9955>提示：</font>默认的，所有HTML元素都有一个静态位置，且无法移动。如需对位置进行操作，要记得首先把元素的 CSS position 属性设置为 relative、fixed 或 absolute！

**操作多个属性**

```javascript
$("button").click(function(){
  $("div").animate({
    left:'250px',
    opacity:'0.5',
    height:'150px',
    width:'150px'
  });
});
```

也可以定义相对值（该值相对于元素的当前值）。需要在值的前面加上 += 或 -=：

```javascript
$("button").click(function(){
  $("div").animate({
    left:'250px',
    height:'+=150px',
    width:'+=150px'
  });
});
```

支持队列功能，会逐一调用队列中的动画

```javascript
$("button").click(function(){
  var div=$("div");
  div.animate({height:'300px',opacity:'0.4'},"slow");
  div.animate({width:'300px',opacity:'0.8'},"slow");
  div.animate({height:'100px',opacity:'0.4'},"slow");
  div.animate({width:'100px',opacity:'0.8'},"slow");
});
```

### 停止动画

**stop() 方法用于在动画或效果完成前对它们进行停止**

**语法**
> $(selector).stop(stopAll,goToEnd);

可选的 stopAll 参数规定是否清除动画队列，默认是 false，只停止活动的动画，队列中的动画会继续执行

可选的 gotoEnd 参数规定是否立即完成当前动画，默认是 false

### Callback 函数

**Callback 函数在当前函数完成之后执行**

```javascript
$("p").hide(1000,function(){
  alert("The paragraph is now hidden");
});
```

### Chaining

**Chaining 允许我们在一条语句中允许多个 jQuery 方法（在相同的元素上）**

```javascript
$("#p1").css("color","red").slideUp(2000).slideDown(2000);
```

## jQuery HTML

### jQuery 获取内容和属性

* text() - 设置或返回所选元素的文本内容
* html() - 设置或返回所选元素的内容（包括 HTML 标记）
* val()  - 设置或返回表单字段的值

```javascript
$("#btn1").click(function(){
  alert("Text: " + $("#test").text());
});
$("#btn2").click(function(){
  alert("HTML: " + $("#test").html());
});
$("#btn2").click(function(){
  alert("Value: " + $("#input").val());
});
```

获得链接中 href 属性的值

```javascript
$("button").click(function(){
  alert($("#w3s").attr("href"));
});
```

### jQuery 设置内容和属性

```javascript
$("#btn1").click(function(){
  $("#test1").text("Hello world!");
});
$("#btn2").click(function(){
  $("#test2").html("<b>Hello world!</b>");
});
$("#btn3").click(function(){
  $("#test3").val("Dolly Duck");
});
```

**text()、html() 以及 val() 的回调函数**

上面的三个 jQuery 方法：text()、html() 以及 val()，同样拥有回调函数。回调函数由两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。

```javascript
$("#btn1").click(function(){
  $("#test1").text(function(i,origText){
    return "Old text: " + origText + " New text: Hello world!
    (index: " + i + ")";
  });
});

$("#btn2").click(function(){
  $("#test2").html(function(i,origText){
    return "Old html: " + origText + " New html: Hello <b>world!</b>
    (index: " + i + ")";
  });
});
```

**设置属性 - attr()**

```javascript
$("button").click(function(){
  $("#w3s").attr({
    "href" : "http://www.w3school.com.cn/jquery",
    "title" : "W3School jQuery Tutorial"
  });
});
```

**attr() 的回调函数**

jQuery 方法 attr()，也提供回调函数。回调函数由两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。

```javascript
$("button").click(function(){
  $("#w3s").attr("href", function(i,origValue){
    return origValue + "/jquery";
  });
});
```

### jQuery 添加元素

* append() - 在被选元素的结尾插入内容
* prepend() - 在被选元素的开头插入内容
* after() - 在被选元素之后插入内容
* before() - 在被选元素之前插入内容

```javascript
$("p").append("Some appended text.");
$("p").prepend("Some prepended text.");
$("img").after("Some text after");
$("img").before("Some text before");

function appendText()
{
var txt1="<p>Text.</p>";               // 以 HTML 创建新元素
var txt2=$("<p></p>").text("Text.");   // 以 jQuery 创建新元素
var txt3=document.createElement("p");  // 以 DOM 创建新元素
txt3.innerHTML="Text.";
$("p").append(txt1,txt2,txt3);         // 追加新元素
}
```

### jQuery 删除元素

* remove() - 删除被选元素（及其子元素）
* empty() - 从被选元素中删除子元素

```javascript
$("#div1").remove();
$("#div1").empty();

删除 class="italic" 的所有 <p> 元素
$("p").remove(".italic");
```

### jQuery 获取并设置 CSS 类

* addClass() - 向被选元素添加一个或多个类
* removeClass() - 从被选元素删除一个或多个类
* toggleClass() - 对被选元素进行添加/删除类的切换操作
* css() - 设置或返回样式属性

```javascript
$("button").click(function(){
  $("h1,h2,p").addClass("blue");
  $("div").addClass("important");
});

$("button").click(function(){
  $("#div1").addClass("important blue");
});

$("button").click(function(){
  $("h1,h2,p").removeClass("blue");
});

$("button").click(function(){
  $("h1,h2,p").toggleClass("blue");
});
```

**css 方法**

> $(selector).css("propertyname"); 返回首个匹配属性的值

> $(selector).css("propertyname","value"); 设置所有匹配的元素的值

> $(selector).css({"propertyname":"value","propertyname":"value",...}); 设置多个 CSS 属性

### jQuery 尺寸

* width() 方法设置或返回元素的宽度（不包括内边距、边框或外边距）
* height() 方法设置或返回元素的高度（不包括内边距、边框或外边距）
* innerWidth() 方法返回元素的宽度（包括内边距）
* innerHeight() 方法返回元素的高度（包括内边距）
* outerWidth() 方法返回元素的宽度（包括内边距和边框）
* outerHeight() 方法返回元素的高度（包括内边距和边框）
* outerWidth(true) 方法返回元素的宽度（包括内边距、边框和外边距）
* outerHeight(true) 方法返回元素的高度（包括内边距、边框和外边距）

方法里面添加参数用于设置尺寸

