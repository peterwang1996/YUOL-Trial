#第二周任务：审计处首页

（完成效果可以[点击这里](http://peterwang1996.github.io/YUOL-Trial/)查看。）

这里是长大在线第二周任务的开发笔记，具体任务是模仿[长江大学审计处首页](http://sjc.yangtzeu.edu.cn/)的主页写一张页面。

##版本更迭

目前这里只有一个版本，即`v1.0`版。有了上一周编写国际学院首页的经验和教训，加上之前也研读过原版的代码，这一次的编写进行得也就相对顺利。基本上是一次成型，没被一些疑难问题困扰得太多。不过，在研读和编写这个页面的时候，也遇到些有意思和可以借鉴的东西，于是放在这里记下，以资日后参考。

###v1.0

####整体布局

在布置这个任务的时候，学长特别地提醒我，这个页面采用的布局是新闻类首页最常用的`1-3-1`式布局，也就是1头+1尾+中间分3列，用代码大概可以这样表达：
```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            #col1, #col2, col3 {
                float: left;
            }
        </style>
    </head>
    <body>
        <div id="header">页面头部</div>
        <div id="middle">
            <!--中间部分，宽度加起来刚好是100%-->
            <div id="col1">主体部分第一列</div>
            <div id="col2">主体部分第二列</div>
            <div id="col3">主体部分第三列</div>
        </div>
        <div id="footer">页面尾部</div>
    </body>
</html>
```
并且还提到说，这种写法可以更好地把页面的结构固定下来，防止某些浏览器（如IE7）相对落后的内核在缩放页面的时候使页面产生变形。这些都是在实践中积累起来的经验，看来我还是得多读书多写页面多总结才行呢。

任务布置下来之后，我又仔细分析了一下那个页面的结构，发现页面头部和尾部的banner，中间主体的四个`<div>`和侧边的两个`<div>`都很相似，它们的代码在很大程度上可以重用。如果仔细规划，代码量可以很大幅度地减少。这样不仅能够使代码更易于阅读和维护，而且也能更节省带宽，提高用户体验。

####关于切图

学长在布置任务的时候，建议我们整页截图过后，自己用PS在图中切取需要的素材。还特别强调说为了使视觉效果最佳，切图的时候一定要精确到像素，否则就对不起PS组的小伙伴们辛辛苦苦做出来的原型了。确实，切图在制作网页时也是一个需要特别注意的环节，马虎不得。而且像我们写的这些需要考虑IE7+兼容性问题的页面，像渐变和圆角矩形这样的元素都无法用纯`CSS`表达，所以切图的时候也要注意将资源尽量留足，避免返工。

每完成一个切片，就可以在切片上单击右键，选择“编辑切片属性”，给切片命名了。这样做的好处无需多言，我们在切图的时候，其实头脑里就开始构思整个页面的架构了。我认为这时候，对照着整个页面给切片命名是最及时的。这样导出切片过后，就可以马上打开编辑器写页面。否则在导出切片之后，看着一堆“零件”给它们起名字，思维就没切图时那么清楚了。

在切图的过程中，我发现了一些问题，那就是我们截下来的整个网页截图中，有一些我们并不需要的元素，比如每个栏目标题图上的“more”字样。而我们手上的资源又没有办法让我们直接把这些字隐藏掉。但是这个时候我想到了用PS CS5以上版本自带的一个名叫“内容识别填充”的工具，在应付这种纯色和简单渐变的场景时特别方便，基本上能一键解决问题。如果我们不知道这个功能，或者在使用老版本的PS，没有这个功能的时候，就要手动截取旁边相似的图案覆盖掉原先的图案，这样相对就麻烦些。所以我们平时也要探索一些常用软件的小技巧，说不定就能在某些时候就能给我们带来意想不到的方便。

然后是选择保存格式的问题。我们知道，Web页面的图片一般有`jpg`、`bmp`和`gif`三种。将所有的切片都保存成了`jpg`格式。我是这样考虑的：`gif`格式最多支持256色，而这个页面包含了大量的渐变元素，用gif势必会导致颜色损失，影响视觉效果；`png`格式固然效果最好，不过文件较大会导致传输变慢，并且在IE的某些版本对`png`格式的支持并不友好（白框bug什么的你懂的），所以也放弃了。综合看来，`jpg`格式应该是最好的选择了。

####HTML&CSS

在这里要着重记下一个新学到的，我个人觉得特别实用的小技巧。我们在做页面的时候，有时会遇到需要将整个页面或者某些元素的背景设置成线性渐变的场景。这在`CSS3`中，用`background: linear-gradient(…);`可以很轻易地解决。但我们写的页面有一个硬性要求就是要兼容IE7+，而IE7是不支持`CSS3`的，所以要另外想办法，这时候貌似用背景图片是唯一的方法了。可是用一大张图片有时会遇到对宽屏设备不友好的情况（图片撑不满整个页面的背景），并且图片消耗带宽，极大影响用户体验。但是在研读原版审计处的代码的时候，发现他们的渐变背景是这样处理的：
```css
body {
    background: url(../images/bg.jpg) repeat-x;
}
```
上面提到的“`bg.jpg`”是一张分辨率为`650*2`的图片。这样的写法在这种场景简直堪称完美，不仅用IE7所支持的`CSS2`就解决了渐变问题，而且这个写法会自动将渐变色铺满整个页面，并且如此体积的图片加载的时间也很短。不得不说这么一招来得是相当巧妙。同时也为我提供了一种解决问题的新思路：如果遇到平铺的元素，也可以用类似这样的方法，截取一个“元”，让它适当重复，以达到节省资源的目的。

然后就是头部banner的问题。在观察原版网页的时候，发现无论页面如何缩放，banner上面的字都是跟下面主题部分的左边缘是对齐的，然而如果把banner和主体写在一个大的`<div>`里面，banner就无法布满整个页面宽。思索了一阵以后，我终于想到在banner中再写一个宽`780px`的`<div>`，并且居中显示，再把字写到那个小`<div>`中，就大功告成了。

####JavaScript

这张网页需要用到`JavaScript`的部分不多，也就是一处需要实时更新的日期/星期模块，看过`Date()`类的文档之后，很快就能写出来了。而且我注意到这样的形式在我们学校的网站中用得很多，于是我把它加入了我的代码片段库中，以便日后使用。

为了提升性能，我仍然把`CSS`和`JavaScript`做了压缩，并且将`JavaScript`做了内嵌。

##感悟

这次的任务完成的时候虽然没有遇到太大的困难，但是一些小技巧确实也让我大开眼界。果然做技术是需要不断积累的。多学习，多实践，多总结，这样才能有效地提高。

##之后的短期学习计划

全面涉猎，重点学习兼容性相关知识，了解服务器相关的基础知识，为下周的任务做准备。

```c
return 0;       //;)
```

------

#补充

完成了审计处首页这个硬性任务之后，剩下几天也不能坐等下一周任务。学长让我们多写多总结，于是我在这几天自己看了一些书，并且写了两个小东西。也发现一些有意思的东西，在这里不太正式地记下。

一是关于编码风格的问题。之前写过别的一些代码，所以我也知道“正三观”这个问题很重要，而且“要从娃娃抓起”。所以看了一些书和别人写的代码，发现这种东西貌似也是有很多“流派”，比如处理防止多人协作时变量重名的时候，有些人喜欢在变量名前面加名字的缩写，有些人喜欢将自己写的代码用一个`(function(){});`包起来；有些人喜欢把一个元素`CSS`写一行，有些人却主张一个属性写一行；还有一些关于`CSS Sprite`，还有`base.css`之类的用法，也是让我大开眼界。究竟这些到底哪些说得是放之四海皆准，或者是需要看情况而定，又或者是类似“甜咸豆腐脑之争”这样无关紧要的事情，也只能交给实践去检验了。

看了极客学院的视频，自己试着写了一个下拉菜单，还是比较容易的。然后再自己的博客模板里小小地发挥了一下，用`CSS3`加了一点动效，其实也只是淡入而已。希望以后学过`jQuery UI`以后，能写一个更加酷炫的下拉效果出来。

仿照[哔哩哔哩](http://bilibili.com/)右侧的边栏的一个按钮做了一个差不多的。用了一点类似`CSS Spirte`的技术，用`JavaScript`结合`setInterval()`不断地改变按钮的`background-position`属性，这样不用Flash也不用`<canvas>`就能完成动画效果，实在是太巧妙了。

尝试不看任何教程，纯凭感觉写了一个定宽的瀑布流效果。中途还是因为对`Array()`类的一些方法不太熟悉，查了不少文档。看来无论是什么样的技术，还是要不断地写，才能对自己用的东西越来越熟悉。在写这个的时候，遇到了Chrome浏览器一个很奇怪的特性，就是Chrome对`offsetWidth` / `offsetHeight`属性的支持不太友好，读取这些属性的时候频频出现问题。然而董康学长提供的源码却没有问题，我调试了许多遍都没调出个所以然，在国内外各种网站也没有找到能够解决问题的方法。这算是一个遗留问题吧，不过我也正在尝试解决，如果走投无路了就只好贴上网求大神指教了。

另外，关于布局，我本来以为这方面我已经掌握的差不多了，结果例会的时候还是被学长问住了。所以关于这方面，我也开始补课了。然而关于“块级格式化上下文”我还是不太理解，今晚下了课结合视频写几个Demo试验一下好了。自从被学长教做人以后，我也越来越不敢说自己学懂了什么东西了，果然是学无止境呢。