# Sass入门学习

## 概念

- 预处理器
    + “CSS 预处理器用一种专门的编程语言，进行 Web 页面样式设计，然后再编译成正常的 CSS 文件，以供项目使用。CSS 预处理器为 CSS 增加一些编程的特性，无需考虑浏览器的兼容性问题”
- 注意
    + Sass 语法是 Sass 的最初语法格式，他是通过 tab 键控制缩进的一种语法规则，而且这种缩进要求非常严格。另外其不带有任何的分号和大括号。常常把这种格式称为 Sass 老版本，其文件名以“.sass”为扩展名
    + SCSS 是 Sass 的新语法格式，从外形上来判断他和 CSS 长得几乎是一模一样，代码都包裹在一对大括号里，并且末尾结束处都有一个分号。其文件名格式常常以“.scss”为扩展名
## 基本语法

- 语法差别

    + Sass 和 CSS 写法的确存在一定的差异，由于 Sass 是基于 Ruby 写出来，所以其延续了 Ruby 的书写规范。在书写 Sass 时不带有大括号和分号，其主要是依靠严格的缩进方式来控制的。

- ```css
    $width : 200px;
    $height : 300px;

    body {
        width:$width;
        height:$height;
    }
    ```

## 安装

- `https://www.sass.hk/install/`

- `Windows`系统下
    + 安装`Ruby`
    + `gem install sass`
- 常用指令
    + ```cmd
        //更新sass
        gem update sass

        //查看sass版本
        sass -v

        //查看sass帮助
        sass -h
        ```
## 编译
- 单文件编译
    + `sass <要编译的Sass文件路径>/style.scss:<要输出CSS文件路径>/style.css`
- 多文件编译
    + `sass sass/:css/`
- 监控
    + `sass --watch <要编译的Sass文件路径>/style.scss:<要输出CSS文件路径>/style.css`

## GUI界面工具编译

## 自动化编译

- `Grunt` 配置 Sass 编译
    + ```javascript
        module.exports = function(grunt) {
        grunt.initConfig({
            pkg: grunt.file.readJSON('package.json'),
            sass: {
                dist: {
                    files: {
                        'style/style.css' : 'sass/style.scss'
                    }
                }
            },
            watch: {
                css: {
                    files: '**/*.scss',
                    tasks: ['sass']
                }
            }
        });
        grunt.loadNpmTasks('grunt-contrib-sass');
        grunt.loadNpmTasks('grunt-contrib-watch');
        grunt.registerTask('default',['watch']);
        }
        ```
- `Gulp` 配置 Sass 编译
    + ```javascript
        var gulp = require('gulp');
        var sass = require('gulp-sass');

        gulp.task('sass', function () {
            gulp.src('./scss/*.scss')
                .pipe(sass())
                .pipe(gulp.dest('./css'));
        });

        gulp.task('watch', function() {
            gulp.watch('scss/*.scss', ['sass']);
        });

        gulp.task('default', ['sass','watch']);
    ```
- `webpack` 配置 Sass
    + 安装依赖`npm i sass-loader`
    + ```javascript
        module.exports = {
            ...
            module:{
                loaders:[
                    ...,
                    {
                    test:/\.scss$/,
                    loader:'style-loader!css-loader!autoprefixer-loader!sass-loader'
                    },
                    ...
                ]
            }
        }
    ```

## 常见编译错误

- 在Sass的编译的过程中，是不是支持“GBK”编码的。所以在创建 Sass 文件时，就需要将文件编码设置为“utf-8”
- 建议在项目中文件命名或者文件目录命名不要使用中文字符。而至于人为失误造成的编译失败，在编译过程中都会有具体的说明，大家可以根据编译器提供的错误信息进行对应的修改

## 输出方式

- ```sass
        //示例
        nav {
            ul {
                margin: 0;
                padding: 0;
                list-style: none;
            }

            li { display: inline-block; }

            a {
                display: block;
                padding: 6px 12px;
                text-decoration: none;
            }
        }
        ```

- 嵌套输出方式`nested`
    + ```css
        nav ul {
            margin: 0;
            padding: 0;
            list-style: none; }
        nav li {
            display: inline-block; }
        nav a {
            display: block;
            padding: 6px 12px;
            text-decoration: none; }
        ```
    + 在编译的时候带上参数“ --style nested”:`sass --watch test.scss:test.css --style nested`
- 嵌套输出方式 `expanded`
    + ```css
        nav ul {
            margin: 0;
            padding: 0;
            list-style: none; 
        }
        nav li {
            display: inline-block; 
        }
        nav a {
            display: block;
            padding: 6px 12px;
            text-decoration: none; 
        }
        ```
    + 在编译的时候带上参数“ --style expanded”: `sass --watch test.scss:test.css --style expanded`
- 嵌套输出方式 `compact`
    + ```css
        nav ul { margin: 0; padding: 0; list-style: none; }
        nav li { display: inline-block; }
        nav a { display: block; padding: 6px 12px; text-decoration: none; }
        ```
    + 在编译的时候带上参数“ --style compact”:`sass --watch test.scss:test.css --style compact`
- 压缩输出方式 `compressed`
    + ```css
        nav ul{margin:0;padding:0;list-style:none}nav li{display:inline-block}nav a{display:block;padding:6px 12px;text-decoration:none}
        ```
    + 压缩输出方式会去掉标准的 Sass 和 CSS 注释及空格。也就是压缩好的 CSS 代码样式风格
    + 在编译的时候带上参数“ --style compressed”:`sass --watch test.scss:test.css --style compressed`

## Sass的基本特性

- 变量声明
    - 1.声明变量的符号“$” 2.变量名称 3.赋予变量的值
        + 如果值后面加上`!default`则表示默认值
- 普通变量与默认变量
    + sass 的默认变量仅需要在值后面加上 `!default` 即可
    + sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可
        - ```sass 
            $baseLineHeight: 2;
            $baseLineHeight: 1.5 !default;
          ```
- 变量调用

- 局部变量和全局变量
    + 全局变量就是定义在元素外面的变量
    + 局部变量就是定义在元素内部的变量
- 嵌套-选择器嵌套
- 嵌套-属性嵌套
    + ```sass
        .box {
            border-top: 1px solid red;
            border-bottom: 1px solid green;
        }
        ```
    + ```sass
        .box {
            border: {
                top: 1px solid red;
                bottom: 1px solid green;
            }
        }
        ```
- 嵌套-伪类嵌套
    + 我们应该尽可能避免选择器嵌套

- 混合宏-声明混合宏
    + `@mixin`
    + 相当于代码片段复用
- 混合宏-调用混合宏
    + `@include`
- 混合宏参数
    + 不带值的参数
        - ```sass
            @mixin border-radius($radius){
                -webkit-border-radius: $radius;
                border-radius: $radius:3px;
            }
            ->
            .box {
                @include border-radius(3px);
            }
            ```
    + 带值的参数
        - ```sass
            @mixin border-radius($radius:3px){
                -webkit-border-radius: $radius;
                border-radius: $radius;
            }
            ->
            .box {
                @include border-radius;
            }
            ```
    + 多个参数
       - ```sass
            @mixin center($width,$height){
                width: $width;
                height: $height;
                position: absolute;
                top: 50%;
                left: 50%;
                margin-top: -($height) / 2;
                margin-left: -($width) / 2;
            }
            ->
            .box-center {
                @include center(500px,300px);
            }
            ```
       - 有一个特别的参数“…”。当混合宏传的参数过多之时，可以使用参数来替代
            + ```sass
                 @mixin box-shadow($shadows...){
                    @if length($shadows) >= 1 {
                        -webkit-box-shadow: $shadows;
                        box-shadow: $shadows;
                    } @else {
                        $shadows: 0 0 2px rgba(#000,.25);
                        -webkit-box-shadow: $shadow;
                        box-shadow: $shadow;
                    }
                }
                ->
                .box {
                    @include box-shadow(0 0 1px rgba(#000,.5),0 0 2px rgba(#000,.2));
                }
                ```
- 混合宏不足
    + 会生成冗余的代码块

- 扩展/继承 @extend
    + ```sass
        //@extend
        .btn {
            border: 1px solid #ccc;
            padding: 6px 10px;
            font-size: 14px;
        }

        .btn-primary {
            background-color: #f36;
            color: #fff;
            @extend .btn;
        }

        .btn-second {
            background-color: orange;
            color: #fff;
            @extend .btn;
        }
        ```
- 占位符 %placeholder
    + %placeholder 声明的代码，如果不被 @extend 调用的话，不会产生任何代码
    + ```sass
        %mt5 {
            magin-top:5px;
        }
        %pt5 {
            magin-top:5px;
        }
        .btn {
            @extend %md5;
            @extend %pt5;
        }
        .block {
            @extend;
            span {
                @extend @pt5
            }
        }
        ```
- 混合宏/继承/占位符 比较使用
    + 如果你的代码块中涉及到变量，建议使用混合宏来创建相同的代码块
    + 如果你的代码块不需要专任何变量参数，而且有一个基类已在文件中存在，那么建议使用 Sass 的继承
    + 占位符是独立定义，不调用的时候是不会在 CSS 中产生任何代码；继承是首先有一个基类存在，不管调用与不调用，基类的样式都将会出现在编译出来的 CSS 代码中

- 插值 #{}
    + #{}语法并不是随处可用，你也不能在 mixin 中调用
    + 可以在 @extend 中使用插值
    + ```sass
        $properties: (margin, padding);
        @mixin set-value($side, $value) {
            @each $prop in $properties {
                #{$prop}-#{$side}: $value;
            }
        }
        .login-box {
            @include set-value(top, 14px);
        }
        ```
- 注释
    + 类似 CSS 的注释方式，使用 ”/* ”开头，结属使用 ”*/ ”，会在编译出来的 CSS 显示
    + 类似 JavaScript 的注释方式，使用“//” ，在编译出来的 CSS 中不会显示
- 数据类型
    + 数字: 如，1、 2、 13、 10px
    + 字符串：有引号字符串或无引号字符串，如，"foo"、 'bar'、 baz
    + 颜色：如，blue、 #04a3f9、 rgba(255,0,0,0.5)
    + 布尔型：如，true、 false
    + 空值：如，null
    + 值列表：用空格或者逗号分开，如，1.5em 1em 0 2em 、 Helvetica, Arial, sans-serif

## Sass运算

- 加法
- 减法
- 乘法
- 除法
    + ”/  ”符号被当作除法运算符时有以下几种情况：
        + 如果数值或它的任意部分是存储在一个变量中或是函数的返回值。
        + 如果数值被圆括号包围。
        + 如果数值是另一个数学表达式的一部分。
- 变量运算
- 数字运算
- 颜色运算
    + ```sass
        p {
            color: #010203 + #040506;
        }
        //计算公式为 01 + 04 = 05、02 + 05 = 07 和 03 + 06 = 09
        ```
- 字符运算
    + 注意，如果有引号的字符串被添加了一个没有引号的字符串 （也就是，带引号的字符串在 + 符号左侧）， 结果会是一个有引号的字符串。 同样的，如果一个没有引号的字符串被添加了一个有引号的字符串 （没有引号的字符串在 + 符号左侧）， 结果将是一个没有引号的字符串

# Sass进阶

- 控制命令
    + `@if`
        - @if 指令是一个 SassScript，它可以根据条件来处理样式块，如果条件为 true 返回一个样式块，反之 false 返回另一个样式块。在 Sass 中除了 @if 之，还可以配合 @else if 和 @else 一起使用
        - ```sass
            @mixin blockOrHidden($boolean:true) {
                @if $boolean {
                    @debug "$boolean is #{$boolean}";
                    display: block;
                    }
                @else {
                    @debug "$boolean is #{$boolean}";
                    display: none;
                    }
            }

            .block {
                @include blockOrHidden;
            }

            .hidden{
                @include blockOrHidden(false);
            }
            ```
    + `@for`
        - @for $i from <start> through <end>
          @for $i from <start> to <end>
        - 这两个的区别是关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数
        - ```sass
            @for $i from 1 through 3 {
                .item-#{$i} { width: 2em * $i; }
            }
            ->
            .item-1 {
                width: 2em;
            }

            .item-2 {
                width: 4em;
            }

            .item-3 {
                width: 6em;
            }
            ```
        - ```sass
            $grid-prefix: span !default;
            $grid-width: 60px !default;
            $grid-gutter: 20px !default;

            %grid {
            float: left;
            margin-left: $grid-gutter / 2;
            margin-right: $grid-gutter / 2;
            }

            //through
            @for $i from 1 through 12 {
            .#{$grid-prefix}#{$i}{
                width: $grid-width * $i + $grid-gutter * ($i - 1);
                @extend %grid;
            }  
            }

            //to
            @for $i from 1 to 13 {
            .#{$grid-prefix}#{$i}{
                width: $grid-width * $i + $grid-gutter * ($i - 1);
                @extend %grid;
            }  
            }
            ```
    + `@while`
        - ```sass
            //SCSS
            $types: 4;
            $type-width: 20px;

            @while $types > 0 {
                .while-#{$types} {
                    width: $type-width + $types;
                }
                $types: $types - 1;
            }
            ```
    + `@each`
        - ```sass
            $list: adam john wynn mason kuroir;//$list 就是一个列表

            @mixin author-images {
                @each $author in $list {
                    .photo-#{$author} {
                        background: url("/images/avatars/#{$author}.png") no-repeat;
                    }
                }
            }

            .author-bio {
                @include author-images;
            }
            ```
- 函数
    + 字符串函数/unquote()
        - `unquote($string)`删除字符串中的引号
        - `quote($string)`给字符串添加引号
            + 使用 quote() 函数只能给字符串增加双引号，而且字符串中间有单引号或者空格时，需要用单引号或双引号括起，否则编译的时候将会报错
    + `To-upper-case()` 函数将字符串小写字母转换成大写字母
    + `To-lower-case()` 函数 与 To-upper-case() 刚好相反，将字符串转换成小写字母
    + 数字函数
        - `percentage($value)`：将一个不带单位的数转换成百分比值
        - `round($value)`：将数值四舍五入，转换成一个最接近的整数
        - `ceil($value)`：将大于自己的小数转换成下一位整数
        - `floor($value)`：将一个数去除他的小数部分
        - `abs($value)`：返回一个数的绝对值
        - `min($numbers…)`：找出几个数值之间的最小值
        - `max($numbers…)`：找出几个数值之间的最大值
        - `random()`: 获取随机数
    + 列表函数
        - `length($list)`：返回一个列表的长度值
            + length() 函数中的列表参数之间使用空格隔开，不能使用逗号，否则函数将会出错
        - `nth($list, $n)`：返回一个列表中指定的某个标签值
            + 在 Sass 中，nth() 函数和其他语言不同，1 是指列表中的第一个标签值，2 是指列给中的第二个标签值
        - `join($list1, $list2, [$separator])`：将两个列给连接在一起，变成一个列表
        - `append($list1, $val, [$separator])`：将某个值放在列表的最后
            + 在 append() 函数中，可以显示的设置 `$separator` 参数
            + 如果取值为 `comma` 将会以逗号分隔列表项
            + 如果取值为 `space` 将会以空格分隔列表项
        - `zip($lists…)`：将几个列表结合成一个多维的列表
            + ```sass
                >> zip(1px 2px 3px,solid dashed dotted,green blue red)
                ((1px "solid" #008000), (2px "dashed" #0000ff), (3px "dotted" #ff0000))
                ```
        - `index($list, $value)`：返回一个值在列表中的位置值
    + Instrospection函数
        - `type-of($value)`：返回一个值的类型
        - `unit($number)`：返回一个值的单位
        - `unitless($number)`：判断一个值是否带有单位
        - `comparable($number-1, $number-2)`：判断两个值是否可以做加、减和合并
    + Miscellaneous函数
        - 他有两个值，当条件成立返回一种值，当条件不成立时返回另一种值
        - `if($condition,$if-true,$if-false)`
    + Map
        - ```sass
            $map: (
                $key1: value1,
                $key2: value2,
                $key3: value3
            )
            ```
    + Maps函数
        - `map-get($map,$key)`：根据给定的 key 值，返回 map 中相关的值
            + ```sass
                $social-colors: (
                    dribble: #ea4c89,
                    facebook: #3b5998,
                    github: #171515,
                    google: #db4437,
                    twitter: #55acee
                );
                .btn-dribble{
                    color: map-get($social-colors,facebook);
                }
                ```
        - `map-merge($map1,$map2)`：将两个 map 合并成一个新的 map
            + 如果 $map1 和 $map2 中有相同的 $key 名，那么将 $map2 中的 $key 会取代 $map1 中的
        - `map-remove($map,$key)`：从 map 中删除一个 key，返回一个新 map
        - `map-keys($map)`：返回 map 中所有的 key
        - `map-values($map)`：返回 map 中所有的 value
        - `map-has-key($map,$key)`：根据给定的 key 值判断 map 是否有对应的 value 值，如果有返回 true，否则返回 false
        - `keywords($args)`：返回一个函数的参数，这个参数可以动态的设置 key 和 value

- 颜色函数
    + `rgb($red,$green,$blue)`：根据红、绿、蓝三个值创建一个颜色
    + `rgba($red,$green,$blue,$alpha)`：根据红、绿、蓝和透明度值创建一个颜色
    + `red($color)`：从一个颜色中获取其中红色值
    + `green($color)`：从一个颜色中获取其中绿色值
    + `blue($color)`：从一个颜色中获取其中蓝色值
    + `mix($color-1,$color-2,[$weight])`：把两种颜色混合在一起
        - `$weight` 为 合并的比例（选择权重），默认值为 50%，其取值范围是 0~1 之间。它是每个 RGB 的百分比来衡量，当然透明度也会有一定的权重。默认的比例是 50%，这意味着两个颜色各占一半，如果指定的比例是 25%，这意味着第一个颜色所占比例为 25%，第二个颜色所占比例为75%
    + HSL函数
        - `hsl($hue,$saturation,$lightness)`：通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色
        - `hsla($hue,$saturation,$lightness,$alpha)`：通过色相（hue）、饱和度(saturation)、亮度（lightness）和透明（alpha）的值创建一个颜色
        - `hue($color)`：从一个颜色中获取色相（hue）值
        - `saturation($color)`：从一个颜色中获取饱和度（saturation）值
        - `lightness($color)`：从一个颜色中获取亮度（lightness）值
        - `adjust-hue($color,$degrees)`：通过改变一个颜色的色相值，创建一个新的颜色
        - `lighten($color,$amount)`：通过改变颜色的亮度值，让颜色变亮，创建一个新的颜色
        - `darken($color,$amount)`：通过改变颜色的亮度值，让颜色变暗，创建一个新的颜色
        - `saturate($color,$amount)`：通过改变颜色的饱和度值，让颜色更饱和，从而创建一个新的颜色
        - `desaturate($color,$amount)`：通过改变颜色的饱和度值，让颜色更少的饱和，从而创建出一个新的颜色
        - `grayscale($color)`：将一个颜色变成灰色，相当于desaturate($color,100%)
        - `complement($color)`：返回一个补充色，相当于adjust-hue($color,180deg)
        - `invert($color)`：反回一个反相色，红、绿、蓝色值倒过来，而透明度不变
    + Opacity函数
        - `alpha($color) /opacity($color)`：获取颜色透明度值
        - `rgba($color, $alpha)`：改变颜色的透明度值
        - `opacify($color, $amount) / fade-in($color, $amount)`：使颜色更不透明
        - `transparentize($color, $amount) / fade-out($color, $amount)`：使颜色更加透明

- @规则
    + `@import`
        - 引入 SCSS 和 Sass 文件。 所有引入的 SCSS 和 Sass 文件都会被合并并输出一个单一的 CSS 文件。 另外，被导入的文件中所定义的变量或 mixins 都可以在主文件中使用
    + `@media`
    + `@extend`
    + `@at-root`
        - 跳出根元素。当你选择器嵌套多层之后，想让某个选择器跳出，此时就可以使用 @at-root
        - ```sass
            .a {
                color: red;

                .b {
                    color: orange;

                    .c {
                    color: yellow;

                        @at-root .d {
                            color: green;
                        }
                    }
                }  
            }
            ->
            .a {
                color: red;
                }

            .a .b {
                color: orange;
                }

            .a .b .c {
                color: yellow;
                }

            .d {
                color: green;
                }
            ```
    + `@debug`
        - @debug 在 Sass 中是用来调试的，当你的在 Sass 的源码中使用了 @debug 指令之后，Sass 代码在编译出错时，在命令终端会输出你设置的提示 Bug
    + `@warn`
        - ```sass
            @mixin adjust-location($x, $y) {
            @if unitless($x) {
                @warn "Assuming #{$x} to be in pixels";
                $x: 1px * $x;
            }
            @if unitless($y) {
                @warn "Assuming #{$y} to be in pixels";
                $y: 1px * $y;
            }
            position: relative; left: $x; top: $y;
            }
            ```
    + `@error`
        - ```sass
            @mixin error($x){
            @if $x < 10 {
                width: $x * 10px;
            } @else if $x == 10 {
                width: $x;
            } @else {
                @error "你需要将#{$x}值设置在10以内的数";
            }

            }

            .test {
            @include error(15);
            }
            ->
            你需要将15值设置在10以内的数 on line 7 at column 5
            ```