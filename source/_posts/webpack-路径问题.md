---
title: webpack-路径问题
date: 2018-04-10 19:53:54
tags: [webpack,路径]
---

# 一、context 

<font color=#A52A2A size=4 >context 是 webpack 编译时的**基础目录**，**webpack会根据此目录去查找入口起点（entry）。**</font>

若不配置，默认值为当前目录，webpack设置 context 默认值[代码](https://github.com/webpack/webpack/blob/master/lib/WebpackOptionsDefaulter.js#L16)：

```javascript
this.set("context", process.cwd());
```

**process.cwd()即webpack运行所在的目录（等同package.json所在目录）。**

context 应该配置为绝对路径，假如入口起点为src/main.js，则可以配置：

``` javascript
{
    context: path.resolve('./src'),
    entry: './main.js'
} 
```

此时 entry 不能再配置为'./src/main.js'，因为 webpack 会相对于 context 配置的 src 目录区查找入口起点（main.js），而 main.js 就在此目录下，所以应当将 entry 配置为当前目录（./）。 

<br/>

context 有什么实际作用？官方文档的解释是使得你的配置独立于工程目录 「This makes your configuration independent from CWD (current working directory)」。怎么理解？举个例子，在分离开发生产配置文件时候，一般把 webpack.config 放到 build 文件夹下，此时 entry 却不用考虑相对于 build 目录来配置，仍然要相对于 context 来配置，这也就是所谓的**独立于工程目录**。 

<br/>

需要注意的是，output 的配置项和 context 没有关系，但是有些插件的配置项和 context 有关，后面会说明。

<br/>

# 二、output

#### 1、output.path

**打包文件输出的目录**，建议配置为绝对路径（相对路径不会报错），**默认值和 context 的默认值一样，都是process.cwd()。**除了常规的配置方式，还可以在 path 中用使用 [hash] 模板，比如配置：

``` javascript
output: {
    path: path.resolve('./dist/[hash:8]/'),
    filename: '[name].js'
 }
```

这里的 hash 值是编译过程的 hash，如果被打包进来的内容改变了，那么 hash 值也会发生改变。这个可以用于版本回滚。为方便做持续集成等，你也可以配置：

```javascript
output: {
    path: path.resolve(`./dist/${Date.now()}/`),
    filename: '[name].js'
 }
```

<br/>

#### 2、ouput.publicPath 

<font color=#A52A2A size=4 >**静态资源最终访问路径 = output.publicPath + 资源loader或插件等配置路径** </font>

```javascript
output.publicPath = '/static/'

// 图片 url-loader 配置
{
    name: 'img/[name].[ext]'
}
// 那么图片最终的访问路径为
// output.publicPath + 'img/[name].[ext]' = '/static/img/[name].[ext]'


// JS output.filename 配置
{
    filename: 'js/[name].js'
}
// 那么JS最终访问路径为 
// output.publicPath + 'js/[name].js' = '/static/js/[name].js'


// CSS 
new ExtractTextPlugin("css/style.css")
// 那么最终CSS的访问路径为
// output.publicPath + 'css/style.css' = '/static/css/style.css'
```

**publicPath 默认值为空字符串**，接下来看其他各种常见的 publicPath 配置的实际意义。 

<br/>

# 三、webpack-dev-server

#### 1、publicPath

我们知道 **webpack-dev-server 打包的内容是放在内存中，通过express匹配请求路径**，然后读取对应的资源输出。**这些资源对外的根目录就是publicPath**，可以理解为和 outpu.path 的功能一样，将所有资源放在此目录下，在浏览器可以直接访问此目录下的资源。 

<br/>

但是这个路径仅仅只是为了提供浏览器访问打包资源的功能，<font color=#A52A2A size=4 >**webpack中的loader和插件仍然是取ouput.publicPath**</font>，比如CSS里面的图片最终的路径仍是"/static/img/xxxx.png"，这也是为什么官方推荐 publicPath 和 webpack 配置的保持一致（除了http地址），配置一致才能正常访问其他静态资源。

<br/>

上面的解释可能还是比较生硬，还是举几个例子来说明。本例将两处 publicPath 配置成不一样的，这样更容易对比理解。

```javascript
// webpack.config.js
output: {
    path: path.resolve(`./dist/`),
    filename: 'js/[name].js',
    publicPath: '/static/'
}
	
// api 调用 webpack-dev-server
var webpack = require('webpack');
var webpackDevServer = require('webpack-dev-server');
var config = require("./webpack.config");
var compiler = webpack(config);
var server = new webpackDevServer(compiler, {
    hot: true,
    publicPath: '/web/'
});
server.listen(8282, "0.0.0.0")
```

如何查看 webpack-dev-server 所有启动后的资源访问路径呢？有个简单的方法，就是访问/webpack-dev-server，本例访问截图如下： 

![](webpack-路径问题\未命名图片.png)

上面的资源可以点击查看，你会发现，资源的路径都是/web/*****，所以在index.html中引入JS的路径应为/web/js/main.js，同时也能看到，style.css中的图片路径仍然为/static/img/****.png，而不是/web/。 

<br/>

# 四、html-webpack-plugin 

这个插件的几处配置受路径配置影响，因此需要专门说明下。 

#### 1、template 

**template的路径是相对于 output.context**，源码如下：

```javascript
this.options.template = this.getFullTemplatePath(this.options.template, compiler.context);
```

因此 template 对应的文件需要放在 ouput.context 配置的目录下才能被识别。

<br/>

#### 2、Filename 

**filename的路径是相对于 output.path**，源码如下：

```javascript
this.options.filename = path.relative(compiler.options.output.path, filename);
```

**在 webpack-dev-server 中，则相对于 webpack-dev-server 配置的 publicPath**。



#### 3、衍生问题

**若 webpack-dev-server 中的 publicPath 和 ouput.publicPath 不一致**，在这种配置下使用html-webpack-plugin是有如下问题：

- 自动引用的路径仍然以 ouput.publicPath      为准，和      webpack-dev-server 提供的资源访问路径不一致，从而不能正常访问； 

- 浏览 index.html 需要加上 webpack-dev-server 配置的 publicPath 才能访问（http://localhost:8282/web/）。

这两个问题也反推出了最方便的配置为：

- **ouput.publicPath 和 webpack-dev-server 的publicPath 均配置为'/'**，vue-cli 就是这种配置
- template 放在根目录，html-webpack-plugin      不用修改参数的路径，filename 采用默认值。

<br/>

# 五、记录曾经用过的配置

```javascript
//webpack.base.config.js
module.exports = {
  entry: {
    // 入口文件的默认路径为context，而不是当前文件路径
    // main: './src/main',
    
    // 使用path.join，从当前文件路径算起，感觉更符合逻辑些
    main: path.join(__dirname, '../src/main'),
    vendors: path.join(__dirname, '../src/vendors')
  },
  output: {
    // 定义了文件build后放置的位置
    path: path.join(__dirname, '../dist/static')
  },
  module: {
    rules:[{
        test: /\.(gif|jpg|png|woff|svg|eot|ttf)\??.*$/,
        loader: 'url-loader?limit=1024',
        query: {
          limit: 10000,
          // 定义了文件的名字，以及相对于output.path路径的位置
		 // build后，引用文件自带img前缀     
		 // 放置在dist/static/img/目录下
          name: 'img/[name].[hash:7].[ext]'
        }
      },
      {  
        test: /\.jpeg$/,
        use: 'url-loader?limit=1024&name=[path][name].[ext]&outputPath=img/&publicPath=output/',
      } 
	]
  },
};



//webpack.prod.config.js
module.exports = merge(webpackBaseConfig, {
  output: {
	 // 定义了index.html中引用js、css、img等资源的引用路径，可以为相对路径或者绝对路径
     publicPath: '/static',
	 // 定义了文件的名字，以及相对于path路径的位置
	 // build后，引用文件自带js前缀     
	 // 放置在dist/static/js/目录下
     filename: 'js/[name].[hash].js',
     chunkFilename: 'js/[name].[hash].chunk.js'
  },
  plugins: [
    new ExtractTextPlugin({
      filename: 'css/[name].[hash].css',
      allChunks: true
    }),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendors',
      filename: 'js/vendors.[hash].js'
    }),
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    }),
    new HtmlWebpackPlugin({
	  // 定义了build后的index.html文件的位置
      filename: path.join(__dirname, '../dist/index_prod.html'),
      template: path.join(__dirname, '../src/template/index.ejs'),
      inject: false
    })
  ]
});
```

