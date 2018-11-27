---
title: gulp入门
date: 2018-11-23 20:44:13
tags: [gulp]
---

## 一、安装gulp和插件

#### 1、安装gulp

```shell
npm install gulp -g
npm install gulp --save-dev
```

<!--more-->

#### 2、常用插件

- sass的编译（[gulp-ruby-sass](https://github.com/sindresorhus/gulp-ruby-sass)）
- 自动添加css前缀（[gulp-autoprefixer](https://github.com/Metrime/gulp-autoprefixer)）
- 压缩css（[gulp-minify-css](https://github.com/jonathanepollack/gulp-minify-css)）
- js代码校验（[gulp-jshint](https://github.com/spenceralger/gulp-jshint)）
- 合并js文件（[gulp-concat](https://github.com/wearefractal/gulp-concat)）
- 压缩js代码（[gulp-uglify](https://github.com/terinjokes/gulp-uglify)）
- 压缩图片（[gulp-imagemin](https://github.com/sindresorhus/gulp-imagemin)）
- 自动刷新页面（[gulp-livereload](https://github.com/vohof/gulp-livereload)）
- 图片缓存，只有图片替换了才压缩（[gulp-cache](https://github.com/jgable/gulp-cache)）
- 更改提醒（[gulp-notify](https://github.com/mikaelbr/gulp-notify)）
- 清除文件（[del](https://www.npmjs.org/package/del)）

```shell
npm install gulp-ruby-sass gulp-autoprefixer gulp-minify-css gulp-jshint gulp-concat gulp-uglify gulp-imagemin gulp-notify gulp-rename gulp-livereload gulp-cache del --save-dev
```

<br/>

### 二、新建`gulpfile.js`

现在，组件安装完毕，我们需要新建`gulpfile`文件以指定`gulp`需要为我们完成什么任务。

在项目根目录新建一个js文件并命名为`gulpfile.j`。

```javascript
// 引入 gulp
var gulp = require('gulp'); 

// 引入组件
var jshint = require('gulp-jshint');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

// 检查脚本
gulp.task('lint', function() {
    gulp.src('./js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// 编译Sass
gulp.task('sass', function() {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});

// 合并，压缩文件
gulp.task('scripts', function() {
    gulp.src('./js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./dist'));
});

// 默认任务
gulp.task('default', function(){
    gulp.run('lint', 'sass', 'scripts');

    // 监听文件变化
    gulp.watch('./js/*.js', function(){
        gulp.run('lint', 'sass', 'scripts');
    });
});
```

注：

**gulp五个常用方法**`task`，`run`，`watch`，`src`，``dest`

<br/>

#### 1、引入组件

```javascript
var gulp = require('gulp'); 

var jshint = require('gulp-jshint');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');
```

我们引入了核心的gulp和其他依赖组件，接下来，分开创建lint, sass, scripts 和 default这四个不同的任务。 

<br/>

#### 2、Lint任务

```javascript
gulp.task('lint', function() {
    gulp.src('./js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});
```

Link任务会检查`js/`目录下得js文件有没有报错或警告。 

<br/>

#### 3、Sass任务

```javascript
gulp.task('sass', function() {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});
```

Sass任务会编译`scss/`目录下的scss文件，并把编译完成的css文件保存到`/css`目录中。 

<br/>

#### 4、Scripts 任务

```javascript
gulp.task('scripts', function() {
    gulp.src('./js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./dist'));
});
```

scripts任务会合并`js/`目录下得所有得js文件并输出到`dist/`目录，然后gulp会重命名、压缩合并的文件，也输出到`dist/`目录。 

<br/>

#### 5、default任务

```javascript
gulp.task('default', function(){
    gulp.run('lint', 'sass', 'scripts');
    gulp.watch('./js/*.js', function(){
        gulp.run('lint', 'sass', 'scripts');
    });
});
```

这时，我们创建了一个基于其他任务的default任务。使用`.run()`方法关联和运行我们上面定义的任务，使用`.watch()`方法去监听指定目录的文件变化，当有文件变化时，会运行回调定义的其他任务。

现在，回到命令行，可以直接运行gulp任务了。

```shell
gulp default
```

<br/>