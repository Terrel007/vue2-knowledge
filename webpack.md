# Webpack配置指南

对`webpack`指南的一个简略版记录，熟悉如何使用`Loaders`和`Plugins`来打包各种资源。

## 加载器(Loaders)

对不同的静态资源(`img`,`fonts`,`css`,`js`,`json`,`Sass`)等使用不同的模块加载器转换成JavaScript模块。

### 管理资源

- **[css-loader](https://github.com/webpack-contrib/css-loader)**：处理JavaScript中模块`import`的CSS文件。

- **[style-loader](https://github.com/webpack-contrib/style-loader)**：处理`css-loader`生成的模块，通过将包含CSS字符串的`<style>`标签插入到html文件的`<head>`中。

- **[file-loader](https://github.com/webpack-contrib/file-loader)**：下载CSS时，CSS样式表中通过`url()`引用`img`,`fonts`类型文件或者在JavaScript模块中`import`图片文件等，可以通过`file-loader`处理，添加到输出目录。在CSS中引用时,`file-loader`会将`url()`中的引用路径替换为输出目录中`img`,`fonts`的路径。

- **[url-loader](https://github.com/webpack-contrib/url-loader)**:类似于`file-loader`,但是如果小于设定的字节数，可以返回DataURL来引用图片等。

- **[csv-loader](https://github.com/theplatapi/csv-loader)**:处理csv数据文件

- **[xml-loader](https://github.com/gisikw/xml-loader)**:处理xml数据文件

  [更多Loader...](https://webpack.js.org/loaders/)

## 插件(Plugins)


### 管理输出

- **[html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)**:打包过程中创建全新的index.html文件，支持默认模板，自动注入`<script>`引用打包后的js文件。

- **[clean-webpack-plugin](https://github.com/johnagan/clean-webpack-plugin)**:清理构建目录(通常为`/dist`文件夹)。

## 开发环境

### 使用`webpack-dev-server`

- **[webpack-dev-server](https://github.com/webpack/webpack-dev-server)**

  - Web dev server: 提供了简单的web服务器，并且能够实时重载(live reloading)。
  ```javascript
  //在webpack配置文件中添加如下配置属性:
  devServer:{
    contentBase:'./dist'
  }
  ```
  - HMR(Hot Module Replacement):译为模块热更新，允许运行时更新各种模块而无需进行完全刷新。

    - 修改webpack配置对象

    ```javascript
    //在webpack配置对象的devServer对象中添加一个`hot:true`的属性
    devServer: {
      contentBase: './dist',
      hot: true
    }
    ```
    - 通过Nodejs api使用

    ```javascript
    //使用webpack-dev-server

    const webpackDevServer = require('webpack-dev-server');
    const webpack = require('webpack');

    const config = require('./webpack.config.js');
    const options = {
      contentBase: './dist',
      hot: true,
      host: 'localhost'
    };

    webpackDevServer.addDevServerEntrypoints(config, options);
    const compiler = webpack(config);
    const server = new webpackDevServer(compiler, options);

    server.listen(5000, 'localhost', () => {
      console.log('dev server listening on port 5000');
    });
    ```


### Node开发服务器中间件
在现有服务器上启用live reloading,HMR等功能。

- **[webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware)**: 是一个容器(Wrapper)，可以把webpack处理后的文件传递给express服务器。

- **[webpack-hot-middleware](https://github.com/glenjamin/webpack-hot-middleware)**:不使用的webpack-dev-server时，允许添加HMR到现存的node服务器。


## 生产环境

### Tree Shaking

> 你可以将应用程序想象成一棵树。绿色表示实际用到的源码和 library，是树上活的树叶。灰色表示无用的代码，是秋天树上枯萎的树叶。为了除去死去的树叶，你必须摇动这棵树，使它们落下。

通常用于描述移除JavaScript上下文中未引用的代码，依赖于ES2015模块系统的静态模块结构特性，如`import`和`export`.

使用Tree Shaking要点:

- 使用ES2015模块语法`import`,`export`

- 引入一个能够删除未使用代码的压缩工具.(如- **[UglifyJSPlugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin)**)


## 代码分离:

三种分离方法:

- 入口点:使用`entry`配置手动分离代码。

- 去重: 使用`CommonsChunkPlugin`去重和分离chunks。

- 动态导入: 通过模块内联函数调用来动态拆分代码。
  1. ECMAScript提案支持的`import()`语法，返回Prmoise。
  2. Webpack支持的`require.ensure`。


## Webpack 4.0的一些重大变更:

### 插件配置
  内置一些插件功能，不再需要安装的插件列表:
  - `NoEmitOnErrorsPlugin` -> `optimization.noEmitOnErrors`,在生产环境默认开启。
  - `ModuleConcatenationPlugin` -> `optimization.concatenateModules`,在生产环境默认开启。
  - `NamedModulesPlugin` -> `optimization.namedModules`,在开发环境默认开启。
  移除`CommonsChunkPlugin`,用`optimization.splitChunks`,`optimization.runtimeChunk`取代。