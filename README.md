# Sass学习

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