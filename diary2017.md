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

## 2017-08-01

> 其实，道理和一件事的投入回报有关，玩游戏是一件低投入，较高回报的事情，而且回报周期非常短。也就是说，人们只要投入很少的精力，就能在很短的时间内获得快乐。但学习却不同，总的来说，学习是一件高投入，高回报的事情，但问题是，学习的回报周期非常长。它需要投入大量的精力，并且需要很长的时间才能看到自己知识水平的提升。

> 实际上我们做选择的时候，理性提醒我们，我们需要做那种有长期回报的事情比如学习，但实际上更多的时候，决定要做什么的不是理性而是感性，那个感性的自我会提醒道，那个回报周期太长了，还是赶快给自己来个爽的吧。于是，遵从于我们的感性本能。大多数人最终选择了那个马上能带来回报的选项，也就是玩游戏。

## 2017-08-02

[CSS秘密花园教程][9]

## 2017-08-06

[技术胖带你玩转ES6视频教程][10]

## 2017-08-10

- 今天用了一上午时间看了两部koa2的教程，收获颇丰嘛。
  - [《Koa 框架教程》][11]
  - [《Koa2进阶学习笔记》][12]

## 2017-08-15

> 归根结底的原因在于，大脑是一个神经网络，神经网络最关键的不是逻辑，而是训练。

## 2017-08-19

> 生活的标准**有时候**并不是以你的资金来定义的，就像你无论多贵的穿戴设备，它并不是改变你生活状态的契机。坚持，并有目标的行动，这才是你可能成功的关键。

## 2017-08-22

[JavaScript秘密花园][13]

## 2017-08-23

今天看了《二十二》，有点气愤，可惜我依稀记得某件事，赶紧找了下记下来，不然可能会真的忘记。

> 1972年，共产党政府与苏联闹翻，急于同美、日建交，为了顺利建交，共产党政府最终在《中日联合声明》中放弃战争赔偿。因此，日本一再拒绝慰安妇的赔偿要求，但中国政府坚称，放弃只是政府放弃，不包括民间索赔。

[Stylus中文参考文档][14]

## 2017-08-25

> 尼玛，今天帮上海一个读国际会计的大二妹子装mac双系统，拿个U盘拷贝win10_lstb镜像到桌面装的时候竟然把u盘格式化了，我38GB资料，果断用`Disk Drill`恢复，哎，还好全部都恢复出来。

![image.png-200.6kB][15]

> 以后装MAC双系统注意，千万注意U盘格式化后被当做挂载装系统问题！

## 2017-09-03

`information ->-> knowledge ->->->->->-> wisdom` 是个长期的累积，并非一朝一夕之功。
  
## 2017-09-05

- 关于多个异步write打开同一个fd读写文件demo

```javascript
var fs = require('fs');
var path = require('path');
process.chdir(__dirname);

var fd = fs.openSync('jerry' + (new Date()).getMilliseconds() + '.txt', 'a');

for (var i = 0; i < 3; i++) {
  var buff = new Buffer('szy' + i);
  var len = buff.length;
  var pos = i * len;
  console.log(buff, pos);
  fs.write(fd, buff, 0, len, pos, function (err, written, string) {
    if (err) {
      console.log('write err');
      return;
    }
    console.log('write ok', written);
  });
}
```

- 但问题是，项目中分片上传大文件合并时会进程无响应呢。

## 2017-09-20

Chrome --> devtools --> Console --> Rendering --> Paint flushing(调试页面是否重绘)

## 2017-09-21

**百元废材人生：就算只值百元 也要给世界一拳。**

如果这场比赛她打赢了，反倒显得特别假。有些人会用这句话来形容这个结局，虽然她失败了，但是她至少努力过。说这句话的人根本没看懂这部电影。
赢一次比赛真的重要吗？她的成功在于重新燃起了对生活的斗志，有了斗志她就是不败的。影片结尾佑二被一子的精神打动，重新认识了一子，想要跟一子开始一段新感情。如果用一句话来总结的话，我觉得佑二来得太迟了。一子32岁开始认识佑二，32岁被佑二刺激，32岁打拳，32岁觉醒。如果她早几年认识了佑二，可能他们的孩子都会打酱油了。

接下来所说高考的事。这几天正式高考技术，我收到很多同学的来信，有人说自己发挥超常，有人说自己发挥失常，有人说自己的朋友高考之后跳楼了，这是一件很悲痛的事。我们从什么时候开始被灌输一个错误的概念：考不上大学，我们的人生就完了。牛叔高考359分，在上大学和参加工作的头几年，我对人生是没有信心的，因为我被灌输的概念是，这个人读书不好，所以你干什么都不行，我处在深刻质疑自己的状态。后来随着我阅历的增长，我发现完全不是这么一回事。我的某些能力根本不是用学历能衡量的，考不上好的大学，我们人生就完了，这句话从两面看，读书好的人，在学校如鱼得水，学习知识建立自信水到渠成；读书不好的人，类似我，对学习没有兴趣，就会质疑自我，直接的结果就是建立不了自我，畏首畏尾，觉得自己做什么事都是错的。人的资质不同，所擅长的能力也不同，凭什么单单用学习来衡量一个人的能力。这句话误导了多少人，阻挡了多少人前进的脚步。咱们说实话，现在这个社会有多少大学生是毕业就失业？有多少大学生是天天在学校躺着，只为了混一个毕业证。父母都希望自己的孩子有出息，大家觉得有毕业证就能出息，还是有能力才能出息？学历是外围，不是必要因素；能力是核心，是必要因素。好的大学真正的意义是更容易提升学生的能力，没考好的同学不要把学历放在心上，这已成事实，没必要遗憾，能力比学历重要得多。你只要记住一点，比你考的好的同学在学校里躺着，你只要稍微努力一点点，以后混的绝对比他好。“考不上好大学，我们的人生就完了”，这是个假命题，如果它挡住了你前进的脚步，让你质疑自己的能力，忘记它，你现在已经有了辨别是否的能力，不要被这句话困住，高考话题结束。

“失败对人生的重要性”

有一个名词叫直升机父母，就是父母害怕子女走弯路，怕受到伤害，所以始终在天空中监视着子女的一言一行，牢牢地掌握在自己手中。这种做法一般有两种结果：第一种，子女承受伤害的能力极其低下，高考没考好跳楼、失恋跳河、父母老师教育两句自杀；第二种，子女被保护的好好的，非常顺利的活了很久，突然有一天发现自己什么都没经历过，问题都是父母解决的，子女一点解决问题的能力都没有，由于长期规避错误，失去了成长的机会，逐渐形成一种假成熟真幼稚的状态。我就实话实说吧，现在很多成年人的内心跟小孩一样，经受不了打击，失业、失恋、离婚、孩子不听话，很容易就造成心理崩溃，承受打击本来是小时候就应该锻炼的能力，结果因为一些原因承受力极其低下。雨果说过一句话，世界上最宽阔的是海洋，比海洋更宽阔的是天空，比天空更宽阔的是人的心灵。内心窄了，一点点问题都是问题，内心宽了，多大的问题都能风平浪静。怎么才能撑开一个人的心灵，马云说过一句话，人的胸怀是被委屈撑大的，我们的心灵之所以狭窄，就是因为经历的事太少了。心灵的磨练是不能躯壳的，胸怀只能被委屈一点点撑大，子女的委屈被父母拦下来了，那么子女留下的只有狭隘，父母对子女的爱是无私的，他们想要让子女过得幸福，幸福的核心是什么，不是有多少钱，也不是不曾受到伤害，幸福源自内心的能量和经历，一个没有智慧没有能量的人，从早上睁开眼睛就是痛苦的。因为他没有解决问题、承受问题的能力，人和人的最大区别其实就是经历。随便说一句，老男人吸引小女孩的状态不就是他的波澜不惊吗？小男孩遇到一点事 噶！抽了，这能行吗？等到有一天你什么问题都不害怕面对，多大的困难都不是困难，你这个还能不幸福吗？金钱不等于幸福，能量等于幸福，智慧等于幸福。一子三十二岁经历了失恋失败，她的人生从此有了动力，面对失败人会有两种反应，经历少的人会觉得天塌了，我努力了这么长时间居然失败了，我的人生失去了前进的动力；经历多的人会觉得这就是我数次失败中的一次，它没什么特殊的，我重整旗鼓重新开始就好，大家仔细想想，经历少的人和经历多的人，哪一种人更容易成功，或者说我们现实生活中，有没有那个人是一次就成功的？她从来没经历过失败。引申出一个问题，有所成就的人，是经历过多次失败最终成功，还是一次就成功。答案显而易见，一次就成功的人自身阅历不足，很容易一夜之间烟消云散，我跟大家说了这么多，总结成一句话：吃亏要趁早，一再回避困难只会让我们失去成长的土壤，没有失败就没有成功，没有阅历就没有智慧，整天躲起来把自己保护得好好的人，最终一定会成为一子那样的行尸走肉，不曾经历过痛苦的人，不会感叹每一分每一秒的美好；不曾经历深夜痛哭的人，不足以谈人生；不曾经历过刻骨铭心愤怒的人，不会对世界大声呐喊；不曾经历过兄弟和女人背叛的人，不会明白人性的丑陋。人生的核心就是经历，不去经历，人生毫无意义。不要躺在床上思考人生，那样找不到正确方向，只会迷失自己。一子父亲用一辈子的悲哀总结出一句话，趁年轻、多尝试、这是好事。小破小立，大破大立，不破不立，不要让别人告诉你，你不行，不要让别人告诉你，这件事你做不了，不要让别人告诉你，你这辈子的成就有多大。有些事我们需要自己去尝试，经历就是人的底蕴，经历就是人的底气，经历就是你跟别人吹牛逼的勇气，经历越多越大，人立的就也高越快，经历越少越小，人立的就越滴越慢。

- 好了，跟大家说一下具体方法：
  1. 读书。毛主席说过一句话，把别人的经验变成自己的，他的本事就大了。有些人通过读书，能把别人的阅历化为自己的，这种读书时真的厉害，这种人的未来潜力无限，但有这种能力的人很少；

  2. 运动。运动能提升一个人的能量，让人散发出阳光。其中的益处无可言表，我这里不多说了。请你别躺在床上质疑我的话，动起来。
  3. 历练。经历这种东西是谁都帮不了你的，我只能告诉你，没有经历不行，不要逃避锻炼自己的机会，你不是朝着成功而去，你是朝着失败而去，用这种心态去面对每一次挑战，你就立于不败之地。

> 买买，全手打呢。

## 2017-09-30

> “相信积累的力量，时间会给我们回馈。”   ---- 《把时间当做朋友》

## 2017-12-12

> 不伴随痛苦的教训是没有意义的。因为人不付出牺牲，就不会有收获。然而当人们克服苦难的时候，会获得不屈服于任何事物的坚强心灵。是的，钢铁般的心。

## 2017-12-20

不管结果如何，为宏大目标而拼搏过的这段经历，将来一定会成为自己的力量。

  [1]: http://golden-layout.com/
  [2]: http://www.cnblogs.com/iyangyuan/p/4190773.html
  [3]: http://static.zybuluo.com/szy0syz/o0jbgevlfcuci7n23vbkaa2y/282221104503701.gif
  [4]: http://static.zybuluo.com/szy0syz/v6et7jp97uniqgt1drd8r0vq/image.png
  [5]: http://static.zybuluo.com/szy0syz/ejist252rt3muul1kgskfhnv/image.png
  [6]: https://segmentfault.com/a/1190000003931851
  [7]: http://read.liuwanlin.info/
  [8]: https://github.com/leizongmin/book-crawler-mysql-cron/blob/master/book.md
  [9]: http://www.w3cplus.com/blog/tags/502.html
  [10]: http://jspang.com/2017/06/03/es6/
  [11]: http://www.ruanyifeng.com/blog/2017/08/koa.html
  [12]: https://github.com/chenshenhai/koa2-note
  [13]: http://bonsaiden.github.io/JavaScript-Garden/zh/
  [14]: http://www.zhangxinxu.com/jq/stylus/
  [15]: http://static.zybuluo.com/szy0syz/5778tc5i6dbnm1zrxs14qdzd/image.png

