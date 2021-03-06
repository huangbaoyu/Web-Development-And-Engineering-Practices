> 本文[响应式开发中合理选定CSS媒体查询分割点]()翻译自David Gilbertson的[The-100%-Correct-Way-To-Do-CSS-breakpoints](https://medium.freecodecamp.com/the-100-correct-way-to-do-css-breakpoints-88d6a5ba1862#.whbc5hodk)一文。本文唔看上去有些拗口，不过其核心是在于给出合适的Media Query命名与编写方式。
> 本文从属于笔者的[Web 前端入门与最佳实践](https://github.com/wxyyxc1992/Web-Frontend-Introduction-And-Best-Practices)中的[Web 响应式开发]()系列文章。


![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/1-l9rwwtUMoiRJOb7uGEkhFg.jpeg)

在阅读本文的时候，反而希望你能先忘却关于CSS、Web开发那些你已经知道的东西，我们今天讨论的并不是多么复杂深奥的内容，如果你觉得准备好了那我们可以从下面这个简单的点图开始:
![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1-XoDgRc5GXaxo7j47ClsIgw.png)
上面这些点分布的有些随意，分分合合，有近有远，我们的问题就是如何将这些点划分入到五个组中。最简单的，我们可以在那些相距较远的两个点之间设置为分隔区划分到不同的组合中。
![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1-cZcTR2tVMzYg1U1h3cqdNg.png)
上面这几个圈都是我随手画出来的，你当然可以选择其他的划分方式，譬如将最右边的两个点划分到一个分组中。其实这个问题并无所谓错误答案，不过如果你以如下方式划分的话:
![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1-RZryP0xAyOy1_WRpBdPIog.png)
看上去是不是觉得怪怪的？我问这个问题也不是无中生有，当我们需要为不同尺寸的屏幕设置不同的CSS样式稿时，会有人喜欢按照最常见的尺寸作为分割点，即320px，768px与1024px。
![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1-pwC0py16i-sQr1agaP26QQ.png)
不知道你有没有听过或者说过下面这些话:中等屏幕的话是不是按照768px来划分？还是应该把768px也划分到中等屏幕的范围内？不过这个尺寸是iPad横屏状态下的尺寸，应该算是大屏幕了吧？唔那大屏幕就是768px和以上尺寸咯？然后320px左右的是小屏幕？那319px算啥？区分蚂蚁的吗？本文的主旨即使讨论如何选择合适的分割点与分隔组。

# 选择合适的分隔点
你在幼儿园里就会画上面的这些圆吧，我现在用矩形度量来详细阐述下:
![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1--ldpo5wcYVnuyRFbO24WPQ.png)
我们在这里选择了600px，900px，1200px以及1800px作为分割点，这些分隔组包含了最常见的14个机型:
![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1-199KbL2oM2P5d4pFMBXYxQ.png)
我们把这两张图合并下，可以得出下面这个更适合你的老板、设计师、开发者以及测试人员看的一张图:
![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1-7YeOvzoYgUEDJdfQy2ERXg.png)

# 用更直观的方式命名你的分组
你愿意的话，也可以使用[Papa-Bear与Baby-Bear](https://css-tricks.com/naming-media-queries/)来称呼你选定的分割点。不过当你和设计师一起讨论网站在不同屏幕上的展示效果时，肯定希望双方都能够在脑海中形成感性直观的认知。如果你用平板竖屏尺寸来形容的话，那到底是iPad还是Surface呢？特别是现在这种手机越来越大，平板越来越小的情况，你很难用单纯的平板或者手机来划分尺寸。不过好消息是苹果已经不做新产品了，他们只是不断地将按钮、耳机口从现在的产品中移除。这边我也不好给出什么建议，只能说设计师和产品之间需要多多沟通。

# 声明式的设置响应式布局

声明式的概念在前端很是流行，譬如著名的React就是典型的声明式组件库。声明式编程应用到CSS中即是CSS应当定义What it wants，而不是How it should。我们上面讨论过的一个关于分割点的容易混淆之处就是分割点同时代表了某个范围。譬如`$large:600px`这种定义在`large`这个词本身代表一个范围值的时候就会混淆。因此在具体的某个组件或者标签中，我们应该对其隐藏具体的尺寸设置，譬如:

```
@mixin for-phone-only {
  @media (max-width: 599px) { @content; }
}
@mixin for-tablet-portrait-up {
  @media (min-width: 600px) { @content; }
}
@mixin for-tablet-portait-only {
  @media (min-width: 600px) and (max-width: 899px) { @content; }
}
@mixin for-tablet-landscape-up {
  @media (min-width: 900px) { @content; }
}
@mixin for-tablet-landscape-only {
  @media (min-width: 900px) and (max-width: 1199px) { @content; }
}
@mixin for-desktop-up {
  @media (min-width: 1200px) { @content; }
}
@mixin for-desktop-only {
  @media (min-width: 1200px) and (max-width: 1799px) { @content; }
}
@mixin for-big-desktop-up {
  @media (min-width: 1800px) { @content; }
}

// usage
.my-box {
  padding: 10px;
  
  @include for-desktop-up {
    padding: 20px;
  }
}
```
你会发现在上面的定义中我很推荐使用`-up`与`-only`后缀，在具体使用样式的地方:
```

.phone-only {
  @include for-phone-only { background: purple; }
}

.tablet-portait-only {
  @include for-tablet-portait-only { background: purple; }
}

.tablet-portrait-up {
  @include for-tablet-portrait-up { background: purple; }
}

.tablet-landscape-only {
  @include for-tablet-landscape-only { background: purple; }
}

.tablet-landscape-up {
  @include for-tablet-landscape-up { background: purple; }
}

.desktop-only {
  @include for-desktop-only { background: purple; }
}

.desktop-up {
  @include for-desktop-up { background: purple; }
}

.big-desktop-up {
  @include for-big-desktop-up { background: purple; }
}
```
不过这种方式在需要大量自定义媒介查询搭配的时候就显得不是那么灵活，我们可以提供更加细粒度的控制方式:
```
@mixin for-size($size) {
  @if $size == phone-only {
    @media (max-width: 599px) { @content; }
  } @else if $size == tablet-portrait-up {
    @media (min-width: 600px) { @content; }
  } @else if $size == tablet-portait-only {
    @media (min-width: 600px) and (max-width: 899px) { @content; }
  } @else if $size == tablet-landscape-up {
    @media (min-width: 900px) { @content; }
  } @else if $size == tablet-landscape-only {
    @media (min-width: 900px) and (max-width: 1199px) { @content; }
  } @else if $size == desktop-up {
    @media (min-width: 1200px) { @content; }
  } @else if $size == desktop-only {
    @media (min-width: 1200px) and (max-width: 1799px) { @content; }
  } @else if $size == big-desktop-up {
    @media (min-width: 1800px) { @content; }
  }
}

// usage
.my-box {
  padding: 10px;
  
  @include for-size(desktop-up) {
    padding: 20px;
  }
}
```
这种方式自然能够达成预期的效果，不过如果某个粗心的开发者传入了某个未预定义的范围名，那么久尴尬了，因此我们还是建议不要传入某个具体的尺寸，而是传入某个范围:
```
@mixin for-size($range) {
  $phone-upper-boundary: 600px;
  $tablet-portrait-upper-boundary: 900px;
  $tablet-landscape-upper-boundary: 1200px;
  $desktop-upper-boundary: 1800px;

  @if $range == phone-only {
    @media (max-width: #{$phone-upper-boundary - 1}) { @content; }
  } @else if $range == tablet-portrait-up {
    @media (min-width: $phone-upper-boundary) { @content; }
  } @else if $range == tablet-portait-only {
    @media (min-width: $phone-upper-boundary) and (max-width: #{$tablet-portrait-upper-boundary - 1}) { @content; }
  } @else if $range == tablet-landscape-up {
    @media (min-width: $tablet-landscape-upper-boundary) { @content; }
  } @else if $range == tablet-landscape-only {
    @media (min-width: $tablet-portrait-upper-boundary) and (max-width: $tablet-landscape-upper-boundary - 1px) { @content; }
  } @else if $range == desktop-up {
    @media (min-width: $tablet-landscape-upper-boundary) { @content; }
  } @else if $range == desktop-only {
    @media (min-width: $tablet-landscape-upper-boundary) and (max-width: $desktop-upper-boundary - 1px) { @content; }
  } @else if $range == big-desktop-up {
    @media (min-width: $desktop-upper-boundary) { @content; }
  }
}

// usage
.my-box {
  padding: 10px;
  
  @include for-size(desktop-up) {
    padding: 20px;
  }
}
```

![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/11/3/4/1-ClU6ZZNLtd0ux8nqRPfhng.png)



























