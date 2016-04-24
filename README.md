
# 介绍
* sugar 是一个创建可继承、可复用和可拓展前端模块 & 组件的轻量级 JavaScript 框架

* 特点：简单优雅的模块化开发方式；视图模块自带模板功能(支持异步请求)和 mvvm 模式

* 项目分为两个独立的部分：**`sugar`** (实现组件模块化) 和 **`svm`** (一个简单的 mvvm 库)

# 框架组成
<img src="http://7xodrz.com1.z0.glb.clouddn.com/sugar-constructor-new" width="666">


# 项目结构
* `demos/` 用 sugar.js 做的一些完整例子

* `test/` 测试文件目录，暂无 unit，都是肉测

* `build/` 打包配置文件目录，打包工具为 webpack

* `dist/` 存放打包好的 sugar.js 和 svm.js 以及各自的压缩版本

* `src/` 源代码文件目录：

	* `src/main/` 为 sugar 的核心模块目录，该目录下所有的模块文件都最终服务于 container.js (视图容器基础模块)，模块之间可以相互创建和消息通信，详细参见: [sugar.md](https://github.com/tangbc/sugar/blob/master/README-sugar.md)

	* **`src/svm/`** 为一个简单 mvvm 库，支持 v-text, v-model, v-bind, v-on 和 v-for 等几个常用的指令，**mvvm 与 sugar 没有任何依赖和耦合，可独立使用**。详细指令参见: [svm.md](https://github.com/tangbc/sugar/blob/master/README-svm.md)


# 举个栗子

```javascript
var sugar = require('dist/sugar.min');

/*
 * 定义 Page 模块，从 sugar.Container (约定了视图模块基础功能和 API) 继承
 * Page 相当于获得了一个独立的视图，展现形式、数据和逻辑行为都可灵活自定义
 */
var Page = sugar.Container.extend({
	init: function(config) {
		// 在 config 里定义模块的初始状态和数据：
		config = this.cover(config, {
			'html'    : '<h1 v-text="title"></h1>',
			// 也可指定视图的模板，sugar 会用 ajax 请求模板地址
			// 'template': 'tpl/page.html',
			'model'   : {
				'title': 'hello sugar~'
			}
		});
		// 调用父类 (sugar.Container) 的 init 方法并传入配置进行视图的渲染
		this.Super('init', arguments);
	},
	// 当视图渲染完毕之后会立即调用自身的 viewReady 方法，可在这里开始业务逻辑
	viewReady: function () {
		this.vm.set('title', 'view has been rendered!');
	}
});

/*
 * 再将定义好的 Page 模块生成实例 mod 并添加到页面
 * mod 是 Page 的一个实例(拥有 Page 的所有属性和方法)
 * 一个 Page 模块可生成 N 个实例，可以近似地认为: mod = new Page();
 */
var mod = sugar.core.create('pageName', Page, {
	'target': document.querySelector('body')
});
```

# 组件示例
**`demos/`**  目录做了些示例，也可在 github.io 上在线预览效果：

* [打星评分组件](http://tangbc.github.io/sugar/demos/star/)
* [简单的日期选择组件](http://tangbc.github.io/sugar/demos/date/)

其他常用组件会后续更新……


# 引用 & 环境
* 引用方式：`sugar.js` 和 `svm.js` 均支持 `cmd` `amd` 以及 `script` 标签引用
	* `sugar (约 41 kb)` http://tangbc.github.io/sugar/dist/sugar.min.js
	* `svm (约 31 kb)` http://tangbc.github.io/sugar/dist/svm.min.js

* 浏览器支持：不支持低版本 IE (用了 `Object.defineProperty` 和 `Function.bind` 等)


# 主要更新日志
* `v1.0`
	* `sugar` 基础的模块/组件系统和创建方式
	* `svm` 支持基础数据模型指令（静态表达式）
* `v2.0`
	* `svm` 支持动态指令表达式: `<p v-text="isError ? errorMsg : sucMsg"></p>`