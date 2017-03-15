---
layout: post
title:  gulp使用配置
date:   2017-03-15 01:08:00 +0800
categories: 工具
tag: 工具
---

* content
{:toc}





gulpfile.js的配置
====================================

{% highlight bash %}
/**
 * Created by ly on 2016/12/5.
 */
//导入工具包 require('node_modules里对应模块')
var gulp = require('gulp'); //本地安装gulp所用到的地方
var fileinclude  = require('gulp-file-include');  //包含HTML
var connect = require('gulp-connect'); //本地服务
var imagemin = require('gulp-imagemin'); //图片压缩
var watch = require('gulp-watch'); //监听
var less = require('gulp-less');
var sass = require('gulp-sass');
var notify = require('gulp-notify');  //处理LESS错误
var plumber = require('gulp-plumber'); //处理LESS错误
var postcss = require('gulp-postcss'); // css 兼容
var autoprefixer = require('autoprefixer'); // 处理浏览器私有前缀
var cssnext = require('cssnext'); // 使用CSS未来的语法
var precss = require('precss'); // 像Sass的函数
var browserSync = require('browser-sync');
var reload = browserSync.reload;
var minifycss = require('gulp-minify-css');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');
var cached = require('gulp-cached');
var remember = require('gulp-remember');
var changed = require('gulp-changed');
var debug = require('gulp-debug');
// .pipe(debug({title: 'translate...:'}))
/*//本地服务
 gulp.task('webserver', function() {
 connect.server({
 port: 8080,      //修改端口
 livereload: true   //添加实时刷新，需要watch相应的css.js,html文件
 });
 });*/

//include html并刷新服务器
gulp.task('fileinclude', function(done) {
    gulp.src(['src/html/*.html'])
        // .pipe(cached('fileinclude'))
        // .pipe(remember('fileinclude'))
        .pipe(fileinclude({
            prefix: '@@',
            basepath: '@file'
        }))
        .pipe(gulp.dest('dist/html'))

        .on('end', done)
        .pipe(reload({ stream: true }));
});

//压缩图片
gulp.task('imagemin', function() {
    gulp.src('src/images/*')
        // 如果想对变动过的文件进行压缩，则使用下面一句代码
        .pipe(cached(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true })))
        .pipe(debug({title: 'translate...:'}))
        .pipe(gulp.dest('dist/images'))
        .pipe(reload({ stream: true }));
});

//打包CSS进入到dist目录
gulp.task('csstodist',  ['testLess', 'testSass'], function() {
    var processors = [autoprefixer, cssnext, precss];
    var stream = gulp.src('src/css/*.css')
            .pipe(cached('csstodist'))
            .pipe(postcss(processors))
            .pipe(remember('csstodist'))
            .pipe(debug({title: 'translate...:'}))
            .pipe(concat('common.css'))
            .pipe(rename({suffix: '.min'}))
            .pipe(minifycss())
            .pipe(gulp.dest('dist/css'))
            .pipe(reload({ stream: true }));
    return stream;
});

gulp.task('testLess', function (callback) {
    gulp.src('src/css/*.less')
        // .pipe(cached('testLess'))
        .pipe(changed('testLess', {extension: '.css'}))
        .pipe(plumber({errorHandler: notify.onError('Error: <%= error.message %>')}))
        .pipe(less())
        // .pipe(debug({title: 'translate...:'}))
        // .pipe(remember('testLess'))
        // .pipe(concat('common_less.css'))
        // .pipe(rename({suffix: '.min'}))
        // .pipe(minifycss())
        .pipe(gulp.dest('src/css'));
        callback();
        // .pipe(reload({ stream: true }));
});

gulp.task('testSass', function (callback) {
    gulp.src('src/css/*.scss')
        // .pipe(cached('testLess'))
        .pipe(changed('testSass', {extension: '.css'}))
        .pipe(sass().on('error', sass.logError))
        // .pipe(debug({title: 'translate...:'}))
        // .pipe(remember('testLess'))
        // .pipe(concat('common_less.css'))
        // .pipe(rename({suffix: '.min'}))
        // .pipe(minifycss())
        .pipe(gulp.dest('src/css'));
        callback();
        // .pipe(reload({ stream: true }));
});

//打包Lib进入到dist目录
gulp.task('Libtodist', function() {
    gulp.src('src/Lib/**/*')
        // .pipe(changed('dist/Lib'))
        .pipe(cached('Libtodist'))
        .pipe(debug({title: 'translate...:'}))
        .pipe(gulp.dest('dist/Lib'))
        .pipe(reload({ stream: true }));
});

//打包JS到dist目录
gulp.task('jstodist', function() {
    gulp.src('src/js/**/*.js')
        // .pipe(cached('jstodist'))
        // .pipe(remember('jstodist'))
        .pipe(changed('dist/js'))
        .pipe(debug({title: 'translate...:'}))
        // .pipe(concat('public.js'))
        // .pipe(rename({suffix: '.min'}))
        // .pipe(uglify())
        .pipe(gulp.dest('dist/js'))
        .pipe(reload({ stream: true }));
});


//打包JS到dist目录
// gulp.task('jscatdist', function() {
//     gulp.src('src/js/*.cat.js')
//         // .pipe(cached('jstodist'))
//         // .pipe(remember('jstodist'))
//         .pipe(debug({title: 'translate...:'}))
//         // .pipe(concat('public.js'))
//         .pipe(rename({suffix: '.min'}))
//         .pipe(uglify())
//         .pipe(gulp.dest('dist/js'))
//         .pipe(reload({ stream: true }));
// });


gulp.task('copyJson', function () {
    gulp.src('src/geoJson/*.*')

        .pipe(gulp.dest('dist/geoJson')).pipe(reload({ stream: true }));
});


// 监视文件改动并重新载入
gulp.task('serve', function() {
    browserSync({
        port: 315,
        server: {
            baseDir: 'dist/'
        }
    });
});

//监听所有HTML JS CSS改动
gulp.task('watch', function () {
   // gulp.watch(['src/css/*','src/css/*.less','src/html/*.html','src/js/*','src/images/*'], ['testLess','jstodist','csstodist','fileinclude','imagemin']);
    gulp.watch(['src/css/*.less'], ['testLess']);
    gulp.watch(['src/css/*.scss'], ['testSass']);
    gulp.watch(['src/css/*.css'], ['csstodist']);
    gulp.watch(['src/Lib/**/*'], ['Libtodist']);
    // gulp.watch(['src/js/public.js'], ['jstodist']);
    gulp.watch(['src/js/*.js'], ['jstodist']);
    gulp.watch(['src/html/**/*.html'], ['fileinclude']);
    gulp.watch(['src/images/*'], ['imagemin']);
    gulp.watch(['src/geoJson/*'], ['copyJson']);
});




gulp.task('default',['csstodist','fileinclude','serve','imagemin','Libtodist','jstodist', 'copyJson','watch']);

{% endhighlight %}



package.json的配置
====================================

{% highlight bash %}
{
  "name": "njga",
  "version": "1.0.0",
  "description": "test",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "autoprefixer": "^6.7.6",
    "browser-sync": "^2.17.3",
    "cssnext": "^1.8.4",
    "gulp": "^3.9.1",
    "gulp-cache": "^0.4.5",
    "gulp-cached": "^1.1.1",
    "gulp-changed": "^2.0.0",
    "gulp-concat": "^2.6.1",
    "gulp-connect": "^5.0.0",
    "gulp-debug": "^3.1.0",
    "gulp-file-include": "^1.0.0",
    "gulp-imagemin": "^3.0.3",
    "gulp-less": "^3.1.0",
    "gulp-minify-css": "^1.2.4",
    "gulp-notify": "^2.2.0",
    "gulp-plumber": "^1.1.0",
    "gulp-postcss": "^6.3.0",
    "gulp-remember": "^0.3.1",
    "gulp-rename": "^1.2.2",
    "gulp-sass": "^3.1.0",
    "gulp-uglify": "^2.0.1",
    "gulp-watch": "^4.3.10",
    "precss": "^1.4.0"
  }
}


{% endhighlight %}
