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

- 嵌套输出方式`nested`
    + 