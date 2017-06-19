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
