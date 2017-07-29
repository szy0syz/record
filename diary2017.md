[TOC]

--------

## 2017/06/12

+ PE镜像工具推荐：[微PE工具箱](http://www.wepe.com.cn/)，有win10内核，原生支持nvme协议。

+ window7登录密码忘记强制重设
  + 进入PE
  + 使用window密码重置程序重设密码并保存
  + 因为window系统自带sam文件校验，会把分区表丢失，使用软件搜索找回分区表，保存
  + 重启验证

## 2017/06/13

- 关于获取当前浏览器可视页面宽和高的记录

```javscript
// 一般来说以下方式获取浏览器可视页面的高度
var curHeight = document.documentElement.clientHeight || document.body.clientHeight

// 但如果说 CSS中加了以下属性后
// html, body { height: 500% }
// document.documentElement.clientHeight 的高度就会被放大5倍
// document.body.clientHeight 的高度还是浏览器可现实页面的高度
```

## 2017-06-15

```css
/*实现文字溢出后出现台阶(...)*/
overflow: hidden;
/*文字超出的拆切部分以'...'代表*/
text-overflow: ellipsis;
white-space: nowrap;
```

+ webstorm中html里，`meta:vp`+`tab`快速移动端布局头
+ 移动端布局小技巧：最外层盒子是不设置宽和高的

## 2017-06-19

> 人为什么要忘记东西呢？又忘了，咋办。

```css
        .box {
            position: absolute; /*让小伙固定死以body为父级参照物*/
            top: 0;
            left: 0;
            height: 100px;
            width: 100px;
            border: 1px solid #000000;
            background-color: lightgreen;
        }
```

- 好吧，有上面这么一个盒子。

```javascript
// box的样式通过样式类名直接赋值，虽然在行内。
box.style.left // -> 按见过仍然是"" -> 空字符串  -> 劳资明明在样式里赋值left0了啊，为哪样是0，坑吧！
// 如果正好拿到left的值的想办法，其实直接domEle.style[xxx]拿到的其实只是一个行内样式，是渲染前的！
// 正确的方式: 通过浏览器的BOM.window.getComputedStyle拿
window.getComputedStyle(box)["left"]
```

- 关于在JavaScript中通过DOM元素拿样式的总结
  1. `dom.style.xxx`，通过这样的方式拿到的***永远是浏览器渲染前的行内样式***！
  2. 可以通过`window.getComputedStyle(xxx)`方法拿到xxx元素在浏览器渲染以后的样式！

- client系列属性总结(重复，为了加强记忆~)：(和内容溢出与否无关)
  - `clientHeight`：容器的内容高度：内容高度+上下padding填充高度。(不包含border宽度)
  - `clientWidth`: 容器的内容宽度：内容宽度+左右padding填充宽度。(不包含border宽度)
  - `clientTop`: 容器上边框的宽度 == `borderTopWidth`
  - `clientLeft`: 容器左边框的宽度 == `borderLeftWidth`

- offset系列属性总结：(和内容溢出与否无关)
  - `offsetHeight`: 整个容器自身高度：内容高度+上下padding填充高度+上下边框宽度
  - `offsetWidth`: 整个容器自身宽度：内容宽度+左右padding填充高度+左右边框宽度
  - `offsetTop`: 从容器上边框外算起(不含容器边框)到父级参照物内边框的距离
  - `offsetLeft`: 从容左上边框外算起(不含容器边框)到父级参照物内边框的距离
  - `offsetParent`: 容器的父级参照物

- scroll系列属性总结：(和内容溢出有关)
  - `scrollHeight`:
    1. 内容无溢出时，`scrollHeight` == `clientHeight`
    2. 内容有溢出时，`scrollHeight`为溢出后内容的真实高度+上下padding填充值
  - `scrollWidth`(内容无溢出): 
    1. 内容无溢出时，`scrollWidth` == `clientWidth`
    2. 内容有溢出时，`scrollWidth`为溢出后内容的真实宽度+左右padding填充值
  - `scrollHeight`和`scrollWidth`两个属性都是返回近似值，因为如果在浏览器中设置`overflow:hidden;`后会对最终返回结果有影响，再者在非标浏览器IE6~8中也会有不同。
  - `scrollTop`: [可写]，当前容器滚动卷去的高度
  - `scrollLeft`: [可写]，当前容器滚动卷去的宽度

## 2017-06-20

> 话由心生：苏轼、佛印、苏小妹。

## 2017-06-22

- 关于JavaScript监听当前窗口是否正被查看中还是没有被查看的小栗子

```javascript
// 兼容性：IE10+，Firefox10+,Chrome14+,Opera12.1+,Safari7.1+
// 在document上先检测有没hidden属性，没有就找webkitHidden，还没有就找mozHidden，最终没有就null
var hiddenProperty = 'hidden' in document ? 'hidden' :    
    'webkitHidden' in document ? 'webkitHidden' :    
    'mozHidden' in document ? 'mozHidden' :    
    null;
// 备好页面是否正被打开字符串"visibilitychange"或"webkitvisibilitychange"或"mozvisibilitychange",这个是dom监听事件句柄名称！
var visibilityChangeEvent = hiddenProperty.replace(/hidden/i, 'visibilitychange');
// handler 监听函数
var onVisibilityChange = function(){
    if (!document[hiddenProperty]) {    
        document.title='窗口正被查看中';
    }else{
        document.title='窗口没被查看中';
    }
}
// 添加dom二级事件
document.addEventListener(visibilityChangeEvent, onVisibilityChange);
```

- 关于异步获取远程数据后，生成元素子节点后绑定数据的坑，异步的坑还真多！

```javascript
var ul = document.getElementById("bannerTip");
var lis = ul.getElementsByTagName("li"); //注意：这里的<li>是根据ajax的异步后的数据生成并绑定的元素，所以说这里的lis是100%的空数组
var xhr = new XMLHttpRequest;
xhr.open("get", "./json/data.json");
xhr.onreadystatechange = function handlerXHR() {
  if (xhr.readyState === 4 && /^2\d\d$/.test(xhr.status)) {
    var data = JSON.parse(xhr.responseText);
    bind(data); //这里进行数据绑定
    lis = ul.getElementsByTagName("li"); // 在这里才能获取到！
  }
}
// 那么问题就来了！就算你闭包再厉害，也是没得元素可供你绑定啊！
~function () {
  for (var i=0; i<lis.length; i++) {
    var cur = lis[i];
    cur.index = i;
    cur.onclick = function () {
    imgIndex = this.index;
    changeTip();
    moveAnimate(inner, {left: -imgIndex*990},500,10);
    }
  }
}();
// 解决办法：把方法绑定在不异步也有的元素上！
ul.onclick = function handlerLiClick(ev) { ... }
```

## 2017-06-23

> 人的记忆是片段的、离散的、不可靠的！ ---- 诺兰【记忆碎片】

- 为什么JavaScript的异步坑那么多！再重复一下异步总结吧！今天被坑在第二条~
    1. 所有定时器都是异步编程；
    2. 所有事件绑定都是异步编程；
    3. 所有ajax请求都是异步编程；
    4. 所有的回调函数都是异步编程；
    
> 在异步编程中开启setInterval定时器是个宇宙级黑洞；真要开启就用dom二级事件绑定，接受可靠的变量地址接收开启的定时器！必须可控啊！

## 2017-06-26

  提高幸福感的两种方法：一种叫做提高幸福所得，另一种叫做降低幸福预期。第一种短期很难提升，第二种则是可以操作的。

## 2017-06-27

- [golden-layout][1] 是非常强大的基于 JavaScript 的 Web 布局工具，它支持窗口的拖拽、缩放以及原生式的弹窗，同时 golden-layout 还提供了丰富的接口以方便动态增删元素、修改布局或者自定义主题。golden-layout 官网还提供了与 RequireJS、React、Angular 等多种其他流行框架协同使用的示例。

## 2017-06-28

- [原生JavaScript事件详解][2]，这个图画得好啊！

![282221104503701.gif-32.3kB][3]

## 2017-06-29

- 负边距-带有右边距的浮动子元素列表

![image.png-5.4kB][4]
看到上面这个图，的第一想法应该是js实现？但其实可以用css实现。不需要单独为每一排最后一个元素单独设`margin-right:0;`。我们可以利用负边距

```html
<style>
.wrap2{
    /*如果把盒子紧贴最右侧，.inner的负边距会体现在浏览器下边的滚动条上，好无奈~~~*/
    /*position: absolute;
    right: 0;*/
    width:322px; /*这里的322px怎么计算的呢？首先是border左右两边各占1px就是2px，然后centent还剩320px，每个小盒子宽各100px就是300px，盒子间间隔10px，需要可见的10px两个就是20px，加起来正好322px。每排最后一个盒子margin-right虽然10px，但不可见。*/
    border:dashed 1px orange;
}

.wrap2 .inner{
  overflow:hidden;
  margin-right:-10px;
}
/*让两个盒子嵌套要显示的那一堆元素，外盒子设置大小，内盒子不设置宽高默认100%，然后设置margin-right为-10px，意思就是让这个盒子右边放大10px，正好把小盒子的margin-right装下*/
.wrap2 .item{
    float:left;
    width:100px;
    height:100px;
    margin:10px 10px 10px 0;
    background:blue;
}
</style>
/*故最终实现原来还是“放大”*/
<div class="wrap2">
    <div class="inner">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
  </div>
</div>
```

![image.png-16.2kB][5]
```html
/*负边距多列布局*/
<style>
.body{
    width:500px;
    margin:10px;
    border:dashed 1px orange;
    overflow:hidden;
}

.wrap3{
    float:left;
    width:100%;
}

.wrap3 .content{
    height:200px;
    margin-right:100px;
    background:rgba(255,0,0,0.5);
}

.body .right{
    width:100px;
    height:200px;
    float:left;
    margin-left:-100px;
    background:rgba(0,255,0,0.5);
}
</style>

<div class="body">
    <div class="wrap3">
        <div class="content">
            Content Content Content Content Content Content Content
            Content Content Content Content Content Content Content Content
        </div>
    </div>
    <div class="right">Right</div>
</div>
```

- 总结【负边距】的应用场景中的作用：
  1. **绝对居中**。容器内某盒子绝对居中定位，例如轮播图左右两个小按钮图的绝对居中定位，可利用负边距。首先轮播图容器relative定位，小按钮容器absolute定位，小按钮top:50%，此时小按钮图的左上角定点定在轮播图容器的中点，但关键小按钮图整个图都还在中线以下。所以补上一个margin-top:-xxpx，此时数值为需要居中容器高度的一半。margin-top负数的作用是把元素在当前位置往上拉xx像素；
  2. **多列布局**。最外层一个框(盒子)的宽度500px，然后我们就有这么一个需求，我们两 左右两个列，右列固定宽为100px，左列宽度是不固定的，为外层盒子宽减右列宽。那么我们的负边距又上场了。外盒子设置500px宽，左列盒子float:left，右列盒子float:left，两者均向左浮动必须为同级。然后给左列盒子的内容不设置宽度但设置一个预设高度，撑开盒子，然后设置一个margin-right:100px，这里也就是为其设置需求右列的固定宽度，意思也就是给它留着一个位置，这样好让右列盒子浮动上来。最后给右列盒子设定高宽，宽度就是需求的宽度，最重要的是设置margin-left:-100px，让本身随着流向左浮动100px，正好浮动上去。
  3. **放大元素**
  4. **带有右边距的浮动子元素列表**

## 2017-06-30

[利用HTML和CSS实现常见的布局][6]

## 2017-07-01

> 在中国传统文化里，不管是匠人还是武师，收徒都要找毫无根基的幼童。这一方面是为了保持师父的绝对权威，方便贯彻落实教学；另一方面是为了尽量延长学徒期，以考察徒弟的品性。拜师之后，徒弟便跟随师父一起生活，经过数年的言传身教，这才得以出师。
然而公司是盈利性的商业组织，不是学校，更不是新手训练营。公司招聘员工的核心诉求，是生产出实实在在、对得起薪水的价值，而不是传承技艺。

## 2017-07-03

- 用原生js/vanilla.js设置元素css属性(left,top...)别忘记加'px'单位。

## 2017-07-04

- [Superlin读书笔记][7]

## 2017-07-12

[网络爬虫与数据库操作][8]

## 2017-07-29

- JavaScript关于引用数据类型的小坑记录

```javascript
var ary = [ {name: 'xx1'}, {name: 'xx2'} ];
var obj = { name: 'xxx3' };
console.dir(ary);
ary[1] = obj;
console.dir(ary); // -> [ {name: 'xx1'}, {name: 'xxx3'} ]
obj = '@@@@@';
console.dir(ary); // -> [ {name: 'xx1'}, {name: 'xxx3'} ]
// 第三次输出ary还是没变，因为当`ary[1] = obj`时，是直接给的引用地址，而后面又`obj = '@@@@@'`时，又给obj一个地址，但ary[1]里记录的地址还是原来的xxx3的地址。所以说最后输出一次还是没变。
```


  [1]: http://golden-layout.com/
  [2]: http://www.cnblogs.com/iyangyuan/p/4190773.html
  [3]: http://static.zybuluo.com/szy0syz/o0jbgevlfcuci7n23vbkaa2y/282221104503701.gif
  [4]: http://static.zybuluo.com/szy0syz/v6et7jp97uniqgt1drd8r0vq/image.png
  [5]: http://static.zybuluo.com/szy0syz/ejist252rt3muul1kgskfhnv/image.png
  [6]: https://segmentfault.com/a/1190000003931851
  [7]: http://read.liuwanlin.info/
  [8]: https://github.com/leizongmin/book-crawler-mysql-cron/blob/master/book.md
