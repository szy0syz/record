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
