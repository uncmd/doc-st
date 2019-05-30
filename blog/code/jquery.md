# jQuery语法

## jQuery 选择器

**选择器允许您对元素组或单个元素进行操作**

### 元素选择器

$("p") 选取 \<p> 元素。

$("p.intro") 选取所有 class="intro" 的 \<p> 元素。

$("p#demo") 选取所有 id="demo" 的 \<p> 元素。

### 属性选择器

$("[href]") 选取所有带有 href 属性的元素。

$("[href='#']") 选取所有带有 href 值等于 "#" 的元素。

$("[href!='#']") 选取所有带有 href 值不等于 "#" 的元素。

$("[href$='.jpg']") 选取所有 href 值以 ".jpg" 结尾的元素。

### CSS选择器

$("p").css("background-color","red");

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