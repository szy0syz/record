## Articles


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
