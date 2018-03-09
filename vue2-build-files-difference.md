# Vue 2.0 中Build文件版本差异

> Refer: `https://github.com/vuejs/vue/blob/dev/dist/README.md`

| | UMD | CommonJS | ES Module |
| --- | --- | --- | --- |
| **Full** | vue.js | vue.common.js | vue.esm.js |
| **Runtime-only** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js |
| **Full (production)** | vue.min.js | | |
| **Runtime-only (production)** | vue.runtime.min.js | | |

## 术语

- **Full**:构建文本包含compiler(模板编译器)和runtime(运行时)代码。

- **Compiler**:负责编译模板字符串为JavaScript渲染函数。

- **Runtime**: 通常是移除Compiler部分代码后的所有代码。包括创建Vue实例，渲染和更新虚拟Dom等等。

- **[UMD](https://github.com/umdjs/umd)**: 基于UMD构建的文件可以直接通过`<script>`标签引入在浏览器中直接使用。


- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS构建版本是为了提供给旧的打包工具如`browserify`和`webpack 1`。

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: ECMAScript模块化构建版本是为了提供给现代化的打包工具，如`webpack 2`和`rollup`。


### Runtime + Compiler vs. Runtime-only

如果需要在JavaScript浏览器运行环境中使用`template`选项指定模板字符串，则需要用Full构建版本。

当使用`vue-loader`或`vueify`时,在构建时,`.vue`文件中的模板就被编译成JavaScript代码了，因此可以使用runtime-only的构建文件。