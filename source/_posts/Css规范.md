---
title: Css规范
---

在一个逐渐膨胀的 CSS 项目中保持样式风格统一是比较困难的：尽管修改一个较小的问题，都可能创建更多丑陋的 hack，也可能 CSS 的小改变会影响 JavaScript 的功能。但是如果我们的项目开始能够遵循一种 css 规则，就能很大程度上避免这些问题。会带来一下这些好处：

1.  更少的样式
2.  更少的样式冲突
3.  更长的维护周期
4.  更快的提升新团队成员
5.  团队成员之间更容易交流
6.  更平稳的项目交接

## BEM 命名规范

BEM 即 Block Element Modifier；类名命名规则： Block\_\_Element--Modifier
BEM 的意思就是块（block）、元素（element）、修饰符（modifier）。

- block: 可以理解为一个区域、一个组件或者一个块级元素，具体如何区分需要根据实际情况具体分析；
- block\_\_element： 就是一个上面的 block 里面的元素，比如说导航（nav：block）里面有 a 标签（a: element）就是一个元素， block 与 element 使用两个下划线链接；
- block\_\_element–modifier：状态或属性。比如 element 里面的 a 标签，它有 active、hover、normal 三种状态，这三种状态就是 modifier。modifier 是使用两个“–”中横线连接

使用两个连字符和下划线而不是一个，是为了让你自己的块可以用单个连字符来界定

```
.sub-block__element {}
.sub-block--modifier {}
```

## BEM 命名法的好处

BEM 的关键是，可以获得更多的描述和更加清晰的结构，从其名字可以知道某个标记的含义。于是，通过查看 HTML 代码中的 class 属性，就能知道元素之间的关联。 ###常规的命名法示例：

```
<div class="article">
    <div class="body">
        <button class="button-primary"></button>
        <button class="button-success"></button>
    </div>
</div>
```

这种写法从 DOM 结构和类命名上可以了解每个元素的意义，但无法明确其真实的层级关系。在 css 定义时，也必须依靠层级选择器来限定约束作用域，以避免跨组件的样式污染。

### 使用了 BEM 命名方法的示例：

```
<div class="article">
    <div class="article__body">
        <div class="tag"></div>
        <button class="article__button--primary"></button>
        <button class="article__button--success"></button>
    </div>
</div>
```

### 在 CSS 预处理器中使用 BEM 格式

BEM 的一个槽点是，命名方式长而难看，书写不雅。相比 BEM 格式带来的便利来说，我们应客观看待。
而且，一般来说会使用通过 LESS/SASS 等预处理器语言来编写 CSS，利用其语言特性书写起来要简单很多。

```
.article {
    max-width: 1200px;

    &__body {
        padding: 20px;
    }

    &__button {
        padding: 5px 8px;

        &--primary {background: blue;}
        &--success {background: green;}
    }
}
```

通过 BEM 命名方式，模块层级关系简单清晰，而且 css 书写上也不必作过多的层级选择。 ###在流行框架的组件中使用 BEM 格式
在当前流行的 Vue.js / React / Angular 等前端框架中，都有 CSS 组件级作用域的编译实现，其基本原理均为利用 CSS 属性选择器特性，为不同的组件生成不同的属性选择器。
当选择了这种局部作用域的写法时，在较小的组件中，BEM 格式可能显得没那么重要。但对于公共的、全局性的模块样式定义，还是推荐使用 BEM 格式。
另外，对于对外发布的公共组件来说，一般为了风格的可定制性，都不会使用这种局部作用域方式来定义组件样式。此时使用 BEM 格式也会大显其彩。 ##命名空间
使用命名空间前缀来进行分类。这些前缀会在你的命名前添加一组字符，但是这个值能立刻标记每一个类的目的，在你看 HTML 或者样式的时候是很需要的。我使用下面这些命名空间：

- 对象类：`.o-`
- 组件：`.c-`
- 状态类：`.is-` 或者 `.has-`
- 主题：`.t-`
- 工具类：`.u-`
- JavaScript 勾子：`.js-`

```
<footer class="c-footer">
    <div class="o-container-wide">
        <a class="c-footer__logo" href="/">The Assist</a>
        <div class="c-social c-social--follow">
            <div class="c-social__label u-txt-center">Join the conversation</div>
            <ul class="c-social__list">
                <li class="c-social__item"></li>
                <li class="c-social__item is-active"></li>
                <li class="c-social__item"></li>
            </ul>
        </div>
        <p class="c-footer__credit"></p>
    </div>
</footer>
```

##属性顺序
HTML 属性应该按照特定的顺序出现以保证易读性。

- class
- id, name
- data-\*
- src, for, type, href, value
- title, alt
- role, aria-\*

Classes 是为高可复用组件设计的，所以他们处在第一位。Ids 更加具体而且应该尽量少使用（例如, 页内书签），所以他们处在第二位。

```
<a class="..." id="..." data-toggle="modal" href="#">
  Example link
</a>

<input class="form-control" type="text">

<img src="..." alt="...">
```

##声明顺序
相关的属性声明应该以下面的顺序分组处理：

- Positioning 相关属性包括：position / top / right / bottom / left / float / display / overflow 等
- Box model 盒模型 相关属性包括：border / margin / padding / width / height 等
- Typographic 相关属性包括：font / line-height / text-align / word-wrap 等
- Visual 外观 相关属性包括：background / color / transition / list-style 等
  Positioning 处在第一位，因为他可以使一个元素脱离正常文本流，并且覆盖盒模型相关的样式。盒模型紧跟其后，因为他决定了一个组件的大小和位置。
  其他属性只在组件 内部 起作用或者不会对前面两种情况的结果产生影响，所以他们排在后面。

```
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
}
```

MDN 上将 CSS 选择符归拆分成四个主要类别，如下所示，性能依次降低。

- ID 规则
- Class 规则
- 标签规则
- 通用规则

### 避免使用后代选择器

不仅性能低下而且代码很脆弱，html 代码和 css 代码严重耦合，html 代码结构发生变化时，CSS 也得修改，特别是在大公司里，写 html 和 css 的往往不是同一个人。

```
html div tr td {..}
```

##使用复合语法
尽可能使用复合语法。

```
.someclass {
 padding-top: 20px;
 padding-bottom: 20px;
 padding-left: 10px;
 padding-right: 10px;
 background: #000;
 background-image: url(../imgs/carrot.png);
 background-position: bottom;
 background-repeat: repeat-x;
}

.someclass {
 padding: 20px 10px 20px 10px;
 background: #000 url(../imgs/carrot.png) repeat-x bottom;
}
```

组织好的代码格式
代码的易读性和易维护性成正比。

```
.someclass-a, .someclass-b, .someclass-c, .someclass-d {
 ...
}

.someclass-a,
.someclass-b,
.someclass-c,
.someclass-d {
 ...
}

.someclass {
    background-image:
        linear-gradient(#000, #ccc),
        linear-gradient(#ccc, #ddd);
    box-shadow:
        2px 2px 2px #000,
        1px 4px 1px 1px #ddd inset;
}
```

## 语法

- 使用两个空格的 soft tabs — 这是保证代码在各种环境下显示一致的唯一方式。
- 使用组合选择器时，保持每个独立的选择器占用一行。
- 为了代码的易读性，在每个声明的左括号前增加一个空格。
- 声明块的右括号应该另起一行。
- 每条声明: 后应该插入一个空格。
- 所有声明应该以分号结尾。虽然最后一条声明后的分号是可选的，但是如果没有他，你的代码会更容易出错。
- 不要在颜色值 rgb(), rgba(), hsl(), hsla(), 和 rect() 中增加空格
- 不要在属性取值或者颜色参数前面添加不必要的 0 (比如，使用 .5 替代 0.5 和 -.5px 替代 0.5px)。
- 所有的十六进制值都应该使用小写字母，例如#fff。因为小写字母有更多样的外形，在浏览文档时，他们能够更轻松的被区分开来。尽可能使用短的十六进制数值，例如使用 #fff 替代 #ffffff。
- 为选择器中得属性取值添加引号，例如 input[type="text"]。 他们只在某些情况下可有可无，所以都使用引号可以增加一致性。
- 不要为 0 指明单位，比如使用 margin: 0; 而不是 margin: 0px;。

```
/* Bad CSS */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0, 0, 0, 0.5);
  box-shadow:0 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}

/* Good CSS */
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin-bottom: 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0px 1px 2px #ccc, inset 0 1px 0 #fff;
}
```

## 不要使用 @import

与 <link> 相比, @import 更慢，需要额外的页面请求，并且可能引发其他的意想不到的问题。应该避免使用他们，而选择其他的方案：
使用多个 <link> 元素
使用 CSS 预处理器例如 Sass 或 Less 将样式编译到一个文件中
使用 Rails, Jekyll 或其他环境提供的功能，来合并 CSS 文件。

##单条声明的声明块
在一个声明块中只包含一条声明的情况下，为了易读性和快速编辑可以考虑移除其中的换行。所有包含多条声明的声明块应该分为多行。
这样做的关键因素是错误检测 - 例如，一个 CSS 验证程序显示你在 183 行有一个语法错误,如果是一个单条声明的行，那就是他了。在多个声明的情况下，你必须为哪里出错了费下脑子。

```
/* Single declarations on one line */
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }

/* Multiple declarations, one per line */
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}
.icon           { background-position: 0 0; }
.icon-home      { background-position: 0 -20px; }
.icon-account   { background-position: 0 -40px; }
```

## 选择器

- 避免在经常出现的组件中使用一些属性选择器 (例如，[class^="..."])。浏览器性能会受到这些情况的影响。
- 减少选择器的长度，每个组合选择器选择器的条目应该尽量控制在 3 个以内。
- 只在必要的情况下使用后代选择器

## 代码组织

以组件为单位组织代码。
制定一个一致的注释层级结构。
当使用多个 CSS 文件时，通过组件而不是页面来区分他们。页面会被重新排列组合，而组件是可以移动的。
