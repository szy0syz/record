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
