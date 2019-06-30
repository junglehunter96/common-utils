##sass的使用

- 建议使用一种语法格式(scss)
- scss sass转换
  - sass-convert main.scss main.sass

###sass变量声明 

- example:

$headline-ff:Braggadocio,Arial,Verdana,Helvetica,sans-serif;

$main-sec-ff:,Arial,Verdana,Helvetica,sans-serif;      

###sass文件引入

@import "variables" //control drective  与默认css@import不同

在编译阶段将引入文件和宿主文件合并输出到相应的css文件中

- 基于sass的既定规则:

  1.没有文件后缀名的时候, sass会添加.scss或者.sass的后缀

  2.同一目录下,局部文件和非局部文件不能重名

###compass引用

- 在宿主文件下建立注释目录 说明引用模块

### 变量操作

1.直接操作变量, 即变量表达式。

2.通过函数。

- function

- mixin

  - @include

  example:

  @mixin col-6 {

  width:50%;

  float:left;

  }

  - @extend:继承类属性

 tip: 

1. extend不可以继承选择器序列

2. 使用%.用来构建只用来继承的选择器

## sass响应式特性

- css media query即媒体查询特性
- sass中的@media跟css区别
  - sass中的media query可以内嵌在css规则中,在生成css的时候, media query才会被提到样式的最高层级
    - 好处: 避免了重复书写选择器或者打乱样式表的流程 

- 在嵌套的时候使用sass的at-root指令 可以将样式输出到样式表的顶层


