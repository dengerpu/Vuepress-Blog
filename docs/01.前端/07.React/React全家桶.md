---
title: React全家桶
index_img: /img/article/empty.png
categories:
  - 分类
tags:
  - 标签1
  - 标签2
date: 2023-11-19 16:25:55
---

## 初始化构建项目

安装create-react-app

```shell
npm i create-react-app -g 「mac需要加sudo」
```

### 新建项目

基于脚手架创建项目「项目名称需要符合npm包规范」

```shell
create-react-app xxx
```

```
|- node_modules  包含安装的模块
|- public  页面模板和IconLogo
    |- favicon.ico
    |- index.html
|- src  我们编写的程序
    |- index.jsx  程序入口「jsx后缀名可以让文件支持jsx语法」
|- package.json
|- ...
```

package.json

```json
{
  ...
  "dependencies": {
    ...
    "react": "^18.2.0",  //核心
    "react-dom": "^18.2.0",  //视图编译
    "react-scripts": "5.0.1", //对打包命令的集成
    "web-vitals": "^2.1.4"  //性能检测工具
  },
  "scripts": {
    "start": "react-scripts start", //开发环境启动web服务进行预览
    "build": "react-scripts build", //生产环境打包部署
    "test": "react-scripts test",   //单元测试
    "eject": "react-scripts eject"  //暴露配置项
  },
  "eslintConfig": {  //ESLint词法检测
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {  //浏览器兼容列表
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

![image-20231119163341520](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191633592.png)

![image-20231119163310514](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191633589.png)

![image-20231119162842505](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191628603.png)

### 暴露配置项

```shell
npm run eject
```

该操作是不可逆的，一旦执行了就不能再恢复了

![image-20231119170055783](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191700887.png)

![image-20231119170338765](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191703832.png)

### 常见的配置修改

#### 配置less

默认安装和配置的是sass，如果需要使用less，则需要：

安装

```shell
yarn add less less-loader@8
```

修改webpack.config.js

```javascript
// 72~73
const lessRegex = /\.less$/;
const lessModuleRegex = /\.module\.less$/;

//507~545
{
  test: lessRegex,
  exclude: lessModuleRegex,
  use: getStyleLoaders(
    ...
    'less-loader'
  )
},
{
  test: lessModuleRegex,
  use: getStyleLoaders(
    ...
    'less-loader'
  ),
}
```

测试

src/index.less

```less
@B: #ccc;

html, body {
  height: 100%;
  background-color: @B;
}
```

在index.js中引入

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import "./index.less"

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <div>
    首页
  </div>
);

```

![image-20231119193059482](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191930558.png)

#### 配置别名

config\webpack.config.js

```javascript
//313
resolve: {
  ...
  alias: {
    '@': paths.appSrc,
    ...
  }
}
```

![image-20231119193904753](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191939804.png)

#### 配置预览域名

scripts/start.js

```javascript
// 47 - 48
const DEFAULT_PORT = parseInt(process.env.PORT, 10) || 8080;
const HOST = process.env.HOST || '127.0.0.1';
```

也可以基于 cross-env 设置环境变量

如果想基于环境变量来修改，需要安装下面的插件

```shell
npm install cross-env
```

![image-20231119184854239](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191848308.png)

```json
  "scripts": {
    "start": "cross-env PORT=8000 HOST=0.0.0.0 node scripts/start.js",
    "build": "node scripts/build.js",
    "test": "node scripts/test.js"
  },
```

补充内容:

cross-env和dot-env

[cross-env 和 dotenv 的使用](https://www.cnblogs.com/ymwmn/p/15952595.html)

`cross-env`和`dotenv`是两个在Node.js项目中常用的工具，它们分别用于处理环境变量的设置，但在功能和使用方式上有一些区别。

##### cross-env

`cross-env` 是一个用于**设置跨平台环境变量的工具**。它的主要作用是确保在不同操作系统上，设置环境变量的语法是一致的。在使用 Node.js 脚本或 npm 脚本时，你可能会在不同的操作系统上面临一些语法差异，例如在 Windows 上使用 `set` 和在类 Unix 系统上使用 `export`。

`cross-env` 可以帮助你规遍这个问题，无论你在哪个平台上运行脚本，都可以使用相同的语法设置环境变量。它不仅可以用于设置 Node.js 环境变量，还可以用于设置其他类型的环境变量。

使用方法：

* 全局安装 `cross-env`：

  ```shell
  npm install -g cross-env
  ```

* 在 package.json 的脚本中使用 `cross-env`：

  ```json
  {
    "scripts": {
      "start": "cross-env NODE_ENV=development node server.js"
    }
  }
  ```

##### dotenv

`dotenv` 是一个用于从文件中加载环境变量的工具。它允许你将环境变量存储在一个名为 `.env` 的文件中，然后在应用程序中加载这些变量。这对于将敏感信息（如 API 密钥）从代码中分离出来，以及在不同环境中配置不同的变量非常有用。

使用方法：

* 安装 `dotenv`：

  ```shell
  npm install dotenv
  ```

* 在应用程序的入口文件（通常是你的主文件，如 `app.js` 或 `index.js`）的顶部加载 `dotenv`：

  ```javascript
  require('dotenv').config();
  ```

* 在项目根目录下创建一个名为 `.env` 的文件，其中包含你的环境变量：

  ```shell
  NODE_ENV=development
  PORT=3000
  API_KEY=your-api-key
  ```

* 在代码中可以直接使用 `process.env` 来访问这些环境变量：

  ```javascript
  const port = process.env.PORT || 3000;
  const apiKey = process.env.API_KEY;
  ```

总体而言，`cross-env` 主要用于解决跨平台的环境变量设置问题，而 `dotenv` 用于从文件中加载环境变量，方便在不同环境中配置和管理。在实际项目中，你可能会同时使用这两个工具来更好地管理环境变量。

#### 配置跨域代理

```shell
yarn add http-proxy-middleware
```

**http-proxy-middleware**:实现跨域代理的模块「webpack-dev-server的跨域代理原理,也是基于它完成的」

在src目录中，新建setupProxy.js

![image-20231119195213074](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191952162.png)

```js
proxySetup: resolveApp('src/setupProxy.js'),
```

> 上述就是为什么要创建在src下创建setupProxy.js的原因

src\setupProxy.js

```javascript
const { createProxyMiddleware } = require("http-proxy-middleware");

module.exports = function (app) {
  app.use(
      createProxyMiddleware("/api", {
          target: "http://127.0.0.1:7100",
          changeOrigin: true,
          ws: true,
          pathRewrite: { "^/api": "" }
      })
  );
  // 下面两个配置测试使用
  app.use(
    createProxyMiddleware("/jianshu", {
        target: "https://www.jianshu.com",
        changeOrigin: true,
        ws: true,
        pathRewrite: { "^/jianshu": "" }
    })
  );
  app.use(
    createProxyMiddleware("/zhihu", {
        target: "https://news-at.zhihu.com/api/4",
        changeOrigin: true,
        ws: true,
        pathRewrite: { "^/zhihu": "" }
    })
  );
};
```

src/index.js调用测试

> //测试地址：
>
> //https://www.jianshu.com/asimov/subscriptions/recommended_collections
>
> //https://news-at.zhihu.com/api/4/news/latest

```javascript
// 跨域测试
fetch("/jianshu/asimov/subscriptions/recommended_collections")
.then(res => res.json())
.then(data => console.log(data))

fetch("/zhihu/news/latest")
.then(res => res.json())
.then(data => console.log(data))
```

![image-20231119200128762](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311192001835.png)

#### 配置浏览器兼容

```json
//package.json
//https://github.com/browserslist/browserslist
"browserslist": {
  "production": [
    ">0.2%",
    "not dead",
    "not op_mini all"
  ],
  "development": [
    "last 1 chrome version",
    "last 1 firefox version",
    "last 1 safari version"
  ]
}
```

![image-20231119185043269](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191850395.png)

##### CSS兼容处理：设置前缀

`autoprefixer + postcss-loader + browserslist`

##### JS兼容处理：ES6语法转换为ES5语法

`babel-loader + babel-preset-react-app(@babel/preset-env) + browserslist`

##### JS兼容处理：内置API

> 正常情况下是使用 `@babel/polyfill`来处理内置API兼容性问题
>
> 然后在入口中`import '@babel/polyfill'`
>
> 脚手架中不需要我们自己去安装: `react-app-polyfill`「对@babel/polyfill的重写」

入口配置`react-app-polyfill`

src/index.js

```js
// ES6内置API做兼容处理
import 'react-app-polyfill/ie9';
import 'react-app-polyfill/ie11';
import 'react-app-polyfill/stable';
```

![image-20231119185216365](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311191852434.png)

## MVC模式和MVVM模式

### 操作DOM方式

```html
  <span id="textBox">0</span><br>
  <button id="submit">累加</button>

  <script>
    //想操作谁，就先获取谁
    let textBox = document.querySelector('#textBox'),
    submit = document.querySelector('#submit');
    // 事件绑定
    let num = 0;
    submit.addEventListener('click' , function () {
      num++;
      textBox.innerHTML = num;
    });
  </script>
```

### React数据驱动格式

```react
import React from 'react';
import ReactDOM from 'react-dom/client';
import "@/index.less"

const root = ReactDOM.createRoot(document.getElementById('root'));

class Count extends React.Component{
  state = {num: 0};
  render() {
    let { num } = this.state;
    return <>
      <span>{num}</span><br/>
      <button onClick={() => {
        num++;
        this.setState({
          num
        })
      }}>累加</button>
    </>
  }
}

root.render(
  <div>
    首页
    <Count/>
  </div>
);
```

### Dom思想和数据驱动思想

主流的思想:不在直接去操作DOM，而是改为“数据驱动思想”操作

#### DOM思想

* 操作DOM比较消耗性能「主要原因就是︰可能会导致DOM重排(回流)/重绘J
* 操作起来也相对来讲麻烦一些

#### 数据驱动思想

* 我们不会在直接操作DOM
* 我们去操作数据「当我们修改了数据，框架会按照相关的数据，让页面重新渲染」
* 框架底层实现视图的渲染,也是基于操作DOM完成的
  * 构建了一套虚拟DOM->真实DOM的渲染体系
  * 有效避免了DOM的重排/重绘
* 开发效率更高、最后的性能也相对较好

### MVC模式和MVVM模式

React框架采用的是MVC体系;

Vue框架采用的是NVVM体系:

#### MVC

![image-20231120150907700](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311201509810.png)

* model数据层
* view视图层
* controller控制层

1. 我们需要按照专业的语法去构建**视图**〔页面):  React中是基于jsx语法来构建视图的
2. 构建**数据层**: 但凡在视图中，需要"动态"处理的(需要变化的，不论是样式还是内容)，我们都要有对应的数据模型
3. **控制层**: 当我们在视图中(或者根据业务需求)进行某些操作的时候，都是去修改相关的数据，然后React枢架会按照最新的数据，重新渲染视图。以此让用户看到最新的效果!

**数据驱动视图的渲染!!**

视图中的表单内容改变,想要修改数据。需要开发者自己去写代码实观!!

**“单向驱动”**

#### MVVM

![image-20231120152800250](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311201528368.png)

* model数据层
* view视图层
* viewModel数据/视图监听层

1. 数据驱动视图的渲染: 监听数据的更新，让视图重新渲染
2. 视图驱动数据的更改: 监听页面中表单元素内容改变。自动去修改相关的数据

**双向驱动**

## JSX

![image-20231120154617124](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311201546206.png)

  JSX：javascript and xml（html）

1. 最外层**只能有一个根元素节点**

2. `<></>` **fragment空标记**，即能作为容器把一堆内容包裹起来，还不占层级结构

3. 动态绑定数据使用`{}`，大括号中存放的是JS表达式

   ​     => 可以直接放数组：把数组中的每一项都呈现出来

   ​     => 一般情况下不能直接渲染对象

   ​    => 但是如果是JSX的虚拟DOM对象，是直接可以渲染的

4. 设置行内样式，必须是 `style={{color:'red'...}}；`

   ​     设置样式类名需要使用的是`className`；

5. JSX中进行的判断一般都要基于**三元运算符**来完成

6.  JSX中遍历数组中的每一项，动态绑定多个JSX元素，一般都是基于数组中的map来实现的

     =>和vue一样，循环绑定的元素要设置key值（作用：用于DOM-DIFF差异化对比）

 JSX语法具备过滤效果（过滤非法内容），有效防止XSS攻击（扩展思考：总结常见的XSS攻击和预防方案？）

> * 在ReactDON.createRoot()的时候，**不能直接把HTML/BODY做为根容器**，需要指定一个额外的盒子「例如:#root
>
> * 每一个构建的视图,只能有一个“根节点”
>
>   * 出现多个根节点则报错Adjacent Jsx elements must be wrapped in an enclosing tag.
>
>   * React给我们提供了一个特殊的节点(标签): **React.Fragment**空文档标记标签
>     `<></>`
>     既保证了可以只有一个根节点。又不新增一个HTML层级结构!!
>
> * JS表达式
>
>   * 变量/值  ` {text}`
>   * 数学运算   `{1+1} -> {2}     {x+y}`
>   * 判断： 三元运算符  `{1===1? 'OK':'NO'}`
>   * 循环：借助于数组的迭代方法处理[`map`]
>
>   表达式中不可以使用`if，else，for, for/in, for/of, while`

### JSX基本使用

+ number/string: 值是啥，就渲染出来啥
+ boolean/null/undefined/Symbol/BigInt︰渲染的内容是空
+ 除数组对象外，其余对象一般都不支持在`{}`中进行渲染，但是也有特殊情况:
  * JSX虚拟DOM对象
  * 给元素设置style行内样式，要求必须写成一个对象格式
+ 数组对象:把数组的每一项都分别拿出来渲染「并不是变为字符串渲染，中间没有逗号」
+ 函数对象:不支持在身中渲染，但是可以作为函数组件，用`<Component/>`方式渲染!!

```react
import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
let text = "首页"
let num = 123
let flag = true
let a = null
let b = undefined
let c = Symbol("1")
let arr = [1,2,3]
let Fun = function() {}
root.render(
  <>
    <span>number/string: 值是啥，就渲染出来啥---{text}, {num}</span><br/>
    <span>boolean/null/undefined/Symbol/BigInt---{flag}, {a}, {b}, {c}</span><br/>
    <span>数组对象:把数组的每一项都分别拿出来渲染---{arr}</span><br/>
    <span>函数对象:不支持在身中渲染，但是可以作为函数组件---<Fun/></span><br/>
  </>
);

```

给元素设置样式

* 行内样式:需要基于对象的格式处理，直接写样式字符串会报错

* 设置样式类名: 需要把class替换为className

```react
    <h2 className='abc' style={{
      color: 'red', 
      fontSize: '24px' // 样式属性要用驼峰命名法
      }}>
      首页
    </h2>
```



![image-20231120170631173](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311201706271.png)

#### 判断元素的显示与隐藏

```react
import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
let flag = false
root.render(
  <>
    {/* 控制元素的display样式:不论显示还是隐藏，元素本身都渲染出来了 */}
    <button style={{
      display: flag ? 'block': 'none'
    }}>控制显示</button>
    <br/>
    {/* 控制元素渲染或者不渲染 */}
    {flag ? <button>控制显示</button> : null}
    <br/>
    {flag && <button>控制显示</button>}
  </>
);
```

#### 数组循环创建元素

```react
import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
let arr = [
  {
  id: 1,
  title: '1111111111111'
  },
  {
    id: 2,
    title: '222222222222'
  },
  {
    id: 3,
    title: '3333333333333333'
  },  
]
root.render(
  <>
   <ul>
    {arr.map((item, index) => {
      // 循环创建的元素一定设置key属性，属性值是本次循环中的“唯一值”「优化DOM-DIFF
      return <li key={item.id}>
        <em>{index}</em>:
        <span>{item.title}</span>
      </li>
    })}
   </ul>
   <ol>
    {/* 要用密集数组，而不能用稀疏数组 */}
    {new Array(5).fill(null).map((_,index) => {
      return <li key={index}>列表{index}</li>
    })}
   </ol>
  </>
);
```

![image-20231121154801806](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311211548887.png)

![image-20231120172353382](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311201723490.png)

![image-20231120172602568](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311201726640.png)

[JavaScript中的稀疏数组与密集数组](https://pythonjishu.com/zdrdnpgklritoym/)

### JSX底层渲染机制

![image-20231121111952947](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311211119097.png)

关于JSX底层处理机制

第一步:把我们编写的JSX语法，编译为**虚拟DOM对象**「virtualDoM」

**虚揪DOM对象**∶框架自己内部构建的一套对象体系(对象的相关成员都是React内部规定的)，基于这些属性描述出，我们所构建视图中的，D节点的相关特征!!

1. 基于` babel-preset-react-app`把JSX编译为 `React.createElement(...)`这种格式!!
   只要是元素节点,必然会基于createElement进行处理!

   * `React.createElement(ele,props,. ..children)`
     * ele: 元素标签名「或组件」
     * props: 元素的属性集合(对象）「如果没有设置过任何的属性，则此值是null」
     * children: 第三个及以后的参数，都是当前元素的子节点

2. 再把 `createElement` 方法执行，创建出virtualDOM虚拟DOM对象「也有称之为:JSX元素、JSX对象、ReactChild对象...」

   ```javascript
   virtualDOM = {
       $$typeof: Symbol( react.element),
       ref: null,
       key: null,
       type: 标签名「或组件」,
       // 存储了元素的相关属性&&子节点信息
       props: {
       	元素的相关属性，
       	children:子节点信息「没有子节点则没有这个属性、属性值可能是一个值、也可能是一个数组」
       }
   }
   ```

第二步:把构建的virtualDOM渲染为**真实DOM**

真实DOM: 浏览器页面中，最后渲染出来，让用户看见的DOM元素! !

```react
// v16
ReactDOM.render(
    <>...</>,
	document.getElementById('root')
);
// v18
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	<>...</>
);

```

> `babel-preset-react-app`
>
> * 对原有@babel/preset-env的重写
>
> * 目的:让其支持JSX语法的编译
>
> 补充说明∶**第一次渲染页面**是直接从**virtualDOM->真实DOM**;但是后期视图更新的时候，需要经过一个**DOM-DIFF**的对比，计算出补丁包PATCH:(两次视图差异的部分)，把PATCH补丁包进行渲染!!

进入这个网站：https://babeljs.io/，选择react可以看到jsx编译后的代码

源代码

```react
import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
let styleObj = {
  color: 'red',
  fontSize: '18px'
}
root.render(
  <>
    <h2 className='abc' style={styleObj}>标题</h2>
    <div className='aaa'>
      <span>a</span>
      <span>b</span>
    </div>
  </>
);
```

编译后的代码

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import { jsx as _jsx } from "react/jsx-runtime";
import { jsxs as _jsxs } from "react/jsx-runtime";
import { Fragment as _Fragment } from "react/jsx-runtime";
const root = ReactDOM.createRoot(document.getElementById('root'));
let styleObj = {
  color: 'red',
  fontSize: '18px'
};
root.render( /*#__PURE__*/_jsxs(_Fragment, {
  children: [/*#__PURE__*/_jsx("h2", {
    className: "abc",
    style: styleObj,
    children: "\u6807\u9898"
  }), /*#__PURE__*/_jsxs("div", {
    className: "aaa",
    children: [/*#__PURE__*/_jsx("span", {
      children: "a"
    }), /*#__PURE__*/_jsx("span", {
      children: "b"
    })]
  })]
}));
```

![image-20231121162017536](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311211620728.png)

下图是视频中编译后的结果，与现在已经有点不太相同了。

![image-20231121162559142](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311211625286.png)

```javascript
console.log(
  React.createElement(
    React.Fragment,null,
    React.createElement(
      "h2",
      { className: "abc", style: styleObj },
      "标题"
    ),
    React.createElement(
      "div" ,
      { className: "aaa" },
      React.createElement("span", null, 'a'),
      React.createElement("span", null, 'b')
    )
  )
)
```

![image-20231121162955092](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311211629211.png)

#### 创建虚拟DOM

```javascript
/**
 * 创建虚拟DOM对象
 * @param {*} ele 元素
 * @param {*} props 属性
 * @param  {...any} children 子节点
 */
export function createElement(ele, props, ...children) {
  let virtualDOM = {
    $$typeof: Symbol('react.element'),
    key: null,
    ref: null,
    type: null,
    props: {}
  };
  let len = children.length;
  virtualDOM.type = ele;
  if(props != null) {
    virtualDOM.props = {
      ...props
    }
  }
  if(len === 1) virtualDOM.props.children = children[0];
  if(len > 0) virtualDOM.props.children = children;
  return virtualDOM;
}
```

#### 虚拟DOM渲染为真实DOM

![image-20231122155732436](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311221557510.png)

封装一个对象迭代的方法 

```javascript
/* 
封装一个对象迭代的方法 
  + 基于传统的for/in循环，会存在一些弊端「性能较差(既可以迭代私有的，也可以迭代公有的)；只能迭代“可枚举、非Symbol类型的”属性...」
  + 解决思路：获取对象所有的私有属性「私有的、不论是否可枚举、不论类型」
    + Object.getOwnPropertyNames(arr) -> 获取对象非Symbol类型的私有属性「无关是否可枚举」
    + Object.getOwnPropertySymbols(arr) -> 获取Symbol类型的私有属性
    获取所有的私有属性：
      let keys = Object.getOwnPropertyNames(arr).concat(Object.getOwnPropertySymbols(arr));
    可以基于ES6中的Reflect.ownKeys代替上述操作「弊端：不兼容IE」
      let keys = Reflect.ownKeys(arr);
*/
const each = function each(obj, callback) {
  if(obj == null || typeof obj !== "object") throw new TypeError("obj is not a object");
  if(typeof callback !== "function") throw new TypeError("callback is not a function");
  let keys = Reflect.ownKeys(obj);
  keys.forEach(key => {
    let value = obj[key];
    // 每一次迭代，都把回调函数执行
    callback(value, key);
  })
}
```

虚拟DOM渲染为真实DOM

```javascript
/**
 * 虚拟DOM渲染为真实DOM
 * @param {*} virtualDOM  
 * @param {*} container 容器
 */
export function render(virtualDOM, container) {
  let {type, props} = virtualDOM;
  if(typeof type === "string") {
    // 动态创建标签
    let element = document.createElement(type);
    each(props, (value, key) => {
      // className的处理：value存储的是样式类名
      if(key === 'className') {
        element.className = value;
        return;
      }
      // style的处理：value存储的是样式对象
      if(key === 'style') {
        let styleObj = value;
        each(styleObj, (val, attr) => {
          element.style[attr] = val;
        })
        return;
      }
      // 子节点的处理：value存储的children属性值
      if(key === 'children') {
        let children = value;
        if(!Array.isArray(children)) children = [children];
        children.forEach(child => {
          // 子节点是文本节点：直接插入即可
          if(/^(string|number)$/.test(typeof child)) {
            element.appendChild(document.createTextNode(child));
            return;
          }
          // 子节点又是一个virtualDOM：递归处理
          render(child, element);
        });
        return;
      }
      element.setAttribute(key, value);
    })
    // 把新增的标签，增加到指定容器中
    container.appendChild(element);
  }
}
```

![image-20231122163829572](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202311221638696.png)

使用

```jsx
import { createElement, render } from './jsxHandle'

let styleObj = {
  color: 'red',
  fontSize: '18px'
}
let virtualDom =   createElement(
  "div",
  {className: "container"},
  createElement(
    "h2",
    { className: "abc", style: styleObj },
    "标题"
  ),
  createElement(
    "div" ,
    { className: "aaa" },
    createElement("span", null, 'a'),
    createElement("span", null, 'b')
  )
)
console.log(virtualDom)
render(virtualDom, document.getElementById("root"));
```

## 组件化开发

> 组件化开发的优势
>
> - 利于团队协作开发
> - 利于组件复用
> - 利于SPA单页面应用开发
> - ……

**Vue中的组件化开发：**

http://fivedodo.com/upload/html/vuejs3/guide/migration/functional-components.html

1. 全局组件和局部组件
2. 函数组件（functional）和类组件「Vue3不具备functional函数组件」

**React中的组件化开发：**

没有明确全局和局部的概念「可以理解为都是局部组件，不过可以把组件注册到React上，这样每个组件中只要导入React即可使用」

1. 函数组件
2. 类组件
3. Hooks组件：在函数组件中使用React Hooks函数

### 函数组件

创建一个函数返回jsx元素

src\views\FunctionComponent.jsx

```jsx
const FunComponent = function (props) {
  console.log("父传子数据", props)
  return <div>
    我是函数组件
  </div>;
}
export default FunComponent;
```

调用组件

src\index.jsx

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import FunComponent from './views/FunctionComponent'

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <>
    <FunComponent title="我是标题" x={10} y="10" data={[100,200]} className="box" style={{ fontSize: '20px'}}/>
    <FunComponent></FunComponent>
  </>
);
```

> 调用组件的时候，我们可以给调用的组件设置(传递)各种各样的属性
>
> `<DemoOne title="我是标题"x={10} data={[100,200]} className="box" style={{ fontSize: '20px'}}/>`
>
> 如果设置的属性值**不是字符串格式**，需要基于{}“胡子语法"进行嵌套

#### 渲染机制

1. 基于babel-preset-react-app把调用的组件转换为createElement格式

   ![image-20231207191323845](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312071913929.png)

   ```javascript
   import { jsx as _jsx } from "react/jsx-runtime";
   /*#__PURE__*/_jsx(FunComponent, {
     title: "\u6211\u662F\u6807\u9898",
     x: 10,
     y: "10",
     data: [100, 200],
     className: "box",
     style: {
       fontSize: '20px'
     }
   });
   ```

   ![image-20231207190625803](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312071906928.png)

   

2. 把createElement方法执行，创建出一个virtualDOM对象!!

   ![image-20231207191303468](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312071913577.png)

   ```javascript
   {
       "key": null,
       "ref": null,
       "props": {
           "title": "我是标题",
           "x": 10,
           "y": "10",
           "data": [
               100,
               200
           ],
           "className": "box",
           "style": {
               "fontSize": "20px"
           }
       },
       "_owner": null,
       "_store": {}
   }
   ```

3. 基于root.render把virtualDOM变为真实的DOM

4. type值不再是一个字符串，而是一个函数了，此时:

   * 把函数执行-> Demo0ne( )
   * 把virtualDOM中的props，作为实参传递给函数-> DemoOne(props)
   * 接收函数执行的返回结果「也就是当前组件的virtualDOM对象」
   * 最后基于render把组件返回的虚拟DOM变为真实DOM，插入到#root容器中!!

#### 扩展：对象规则设置

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import FunComponent from './views/FunctionComponent'

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <>
    <FunComponent title="我是标题" x={10} y="10" data={[100,200]} className="box" style={{ fontSize: '20px'}}/>
    <FunComponent></FunComponent>
  </>
);

// 对象规则
const obj = {x: 1, y: 2, z: 3};
// Object.freeze(obj)
// console.log(Object.isFrozen(obj)) // 判断一个对象是否被冻结
// 冻结后，不能新增，不能修改，不能删除，不能劫持
// obj.x = 1111;
// obj.a = 2222;
// delete obj.z;
// Object.defineProperty(obj, 'x', {
//   get() {},
//   set() {}
// })

// 密封对象
// Object.seal(obj);
// // 密封后，可以修改，不能新增，不能删除
// obj.x = 15; // 可修改
// obj.a = 2222; // 不能新增
// delete obj.z; // 不能删除
// Object.defineProperty(obj, 'x', { // 不可以被重新定义
//   get() {},
//   set() {}
// })

// 不可扩展
Object.preventExtensions(obj);
// 可修改，可删除，不可新增，不可以被重新定义
obj.x = 1111; // 可修改
// obj.a = 333;  // 不可新增
delete obj.y; // 可删除
Object.defineProperty(obj, 'x', { // 可以被重新定义
  get() {},
  set() {}
})

console.log(obj);
```

扫盲知识点:关于对象的规则设置

* **冻结**
  * 冻结对象:	`0bject.freeze(obj)`
  * 检测是否被冻结: `0bject.isFrozen(obj) =>true/false`
  * 被冻结的对象︰**不能修改成员值**、**不能新增成员**、**不能删除现有成员**、**不能给成员做劫持**「Object.defineProperty
* **密封**
  * 密封对象:	`0bject.seal(obj)`
  * 检测是否被密封:` 0bject.isSealed(obj)`
  * 被密封的对象∶**可以修改成员的值**，但也**不能删、不能新增、不能劫持**!!
* **扩展**
  * 把对象设置为不可扩展: `0bject.preventExtensions(obj)`
  * 检测是否可扩展: `0bject.isExtensible(obj)`
  * 被设置不可扩展的对象: 除了**不能新增成员**、其余的操作都可以处理!!

> 被冻结的对象，即是不可扩展的，也是密封的! !同理，被密封的对象，也是不可扩展的! !'

#### 属性props处理

​	调用组件，**传递进来的属性是“只读"的**「原理: **props对象被冻结了**」`object.isFrozen(props) -> true`

* 获取:props.xxX
* 修改:props.xxX=xxx=→>报错

作用:父组件(index.jsx)调用子组件(DemoOne. jsx)的时候，可以基于属性，把不同的信息传递给子组件;

子组件接收相应的属性值，呈现出不同的效果，让组件的复用性更强!!

##### 规则校验

虽然对于传递进来的属性，我们不能直接修改，但是可以做一些规则校验

* 设置默认值

```javascript
// 设置默认值
函数组件名称.defaultProps = {
  x: 1,
  y: 1,
  title: "默认标题"
}
```

* 设置其他规则

  需要依赖于一个插件  `prop-types`

  ```shell
  npm install prop-types
  ```

  使用说明：https://github.com/facebook/prop-types

  ```javascript
  import PropTypes from 'prop-types';
  // 设置其他校验规则
  函数组件名称.propTypes = {
    // 类型字符串，必传
    title: PropTypes.string.isRequired,
    // 类型是数字
    x: PropTypes.number,
    // 类型是字符串
    y: PropTypes.string,
    // 几种类型中的一种
    z: PropTypes.oneOfType([
      PropTypes.number,
      PropTypes.string
    ]),
    data: PropTypes.arrayOf(PropTypes.number)
  }
  ```

  传递进来的属性，首先会经历规则的校验，不管校验成功还是失败，最后都会把属性给形参props，只不过如果不符合设定的规则，**控制台会抛出警告错误[不影响属性值的获取]**!!

  > 如果就想把传递的属性值进行修改，我们可以:
  >
  > * 把props中的某个属性赋值给其他内容「例如:变量、状态...J
  > * 我们不直接操作props.xxX=XXx，但是我们可以修改变量/状态值!!
  >
  > ​	 // 可以通过这种方式实现对传过来的数据值的修改
  >
  >  	`let z = props.z`
  >
  > ​	` z = 123`

#### 属性children

![image-20231207195731253](C:\Users\Administrator.SC-201902031211\AppData\Roaming\Typora\typora-user-images\image-20231207195731253.png)

```javascript
  // 要对children的类型做处理
  // 可以基于React.Children对象中提供的方法，对props.children做处理:count\forEach\map\toArray...
  // 好处:在这些方法的内部，已经对children的各种形式做了处理
  children = React.Children.toArray(children);
  // if(!children) {
  //   children = []
  // } else if (!Array.isArray(children)) {
  //   children = [children]
  // }
```

> 如果组件上面也传递了chidren属性，并且组件内部也传递了children，组件内部传递的数据会覆盖组件上面传递的
>
> ```jsx
>     <FunComponent title="我是标题" children={[100,200]} x={10} y="10" z={122} data={[100,200]} className="box" style={{ fontSize: '20px'}}>
>       <span style={{color: "red"}}>1111</span>
>     </FunComponent>
> ```
>
> ![image-20231208215634847](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312082156967.png)
>
> 如果注释掉`<span style={{color: "red"}}>1111</span>`，则children输出为`[100,200]`

#### 具名插槽

```jsx
import React from 'react';

const SlotComponent = function (props) {
  let {children} = props;
  children = React.Children.toArray(children);
  let headerSlot = [],
  contentSlot = [],
  footerSlot = [];
  // headSlots = children.filter(item => item.props.slot === 'head'),
  // footSlots = children.filter(item => item.props.slot === 'foot');
  children.forEach(child => {
    // 传递进来的插槽信息，都是编译为virtualDOM后传递进来的「签」
    let {slot} = child.props;
    if(slot === 'header') {
      headerSlot.push(child)
    } else if (slot === 'content') {
      contentSlot.push(child);
    } else {
      footerSlot.push(child);
    }
  })
  return <div>
    {headerSlot}
    <hr/>
    {contentSlot}
    <br />
    {footerSlot}
  </div>;
}
export default SlotComponent;
```

调用

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import SlotComponent from './views/SlotComponent';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <>
    {/* 具名插槽使用 */}
    <SlotComponent>
      <h2 slot="header">我是标题</h2>
      <p slot="footer">我是页脚</p>
      <div slot='content'>
        <div style={{width: 300, height: 300, border: '1px solid red'}}>
          我是内容区域
        </div>
      </div>
    </SlotComponent>
  </>
);
```

![image-20231207201735075](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312072017330.png)

#### 封装组件Dialog

Dialog.jsx

```jsx
import React from 'react';
import Style from './css/styles.module.scss';
import PropTypes from 'prop-types'
const Dialog = function (props) {
  let {title, children} = props;
  children = React.Children.toArray(children);
  return <div className={Style.dialog_container}>
    <div className={Style.dialog_container_header}>
      <span>{title}</span>
      <span>x</span>
    </div>
    <div className={Style.dialog_container_content}>
      {children}
    </div>
    <div className={Style.dialog_container_footer}>
      <button>确定</button>
      <button>关闭</button>
    </div>
  </div>
}

Dialog.defaultProps = {
  title: '温馨提示'
}

Dialog.propTypes = {
  title: PropTypes.string
}

export default Dialog
```

src\views\css\styles.module.scss

```scss
.dialog_container {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  .dialog_container_header {
    height: 10%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 5px;
    border-bottom: 1px solid #ccc;
    span:nth-child(2) {
      cursor: pointer;
    }
  }
  .dialog_container_content {
    height: 80%;
  }
  .dialog_container_footer {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    height: 10%;
    border-top: 1px solid #eee;
    button {
      margin: 0 5px;
    }
  }
}
```

![image-20231207203127698](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312072031805.png)

调用

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import Dialog from './views/Dialog';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <>
    <Dialog title="提示框">
      <div style={{display: 'flex', justifyContent: 'center'}}>
        我是里面的内容
      </div>
    </Dialog>
  </>
);
```

### 静态组件

函数组件是`静态组件`

- 不具备状态、生命周期函数、ref等内容
- 第一次渲染完毕，除非父组件控制其重新渲染，否则内容不会再更新
- 优势：渲染速度快
- 弊端：静态组件，无法实现组件动态更新

```jsx
/* 
 函数组件是“静态组件”
   第一次渲染组件，把函数执行
     + 产生一个私有的上下文：EC(V)
     + 把解析出来的props「含children」传递进来「但是被冻结了」
     + 对函数返回的JSX元素「virtualDOM」进行渲染
   当我们点击按钮的时候，会把绑定的小函数执行：
     + 修改上级上下文EC(V)中的变量
     + 私有变量值发生了改变
     + 但是“视图不会更新”
   =>也就是，函数组件第一次渲染完毕后，组件中的内容，不会根据组件内的某些操作，再进行更新，所以称它为静态组件
   =>除非在父组件中，重新调用这个函数组件「可以传递不同的属性信息」

 真实项目中，有这样的需求：第一次渲染就不会再变化的，可以使用函数组件！！
 但是大部分需求，都需要在第一次渲染完毕后，基于组件内部的某些操作，让组件可以更新，以此呈现出不同的效果！！==> 动态组件「方法：类组件、Hooks组件(在函数组件中，使用Hooks函数)」
 */
 const Vote = function Vote(props) {
  let { title } = props;
  let supNum = 10,
      oppNum = 5;

  return <div className="vote-box">
      <div className="header">
          <h2 className="title">{title}</h2>
          <span>{supNum + oppNum}人</span>
      </div>
      <div className="main">
          <p>支持人数：{supNum}人</p>
          <p>反对人数：{oppNum}人</p>
      </div>
      <div className="footer">
          <button onClick={() => {
              supNum++;
              console.log(supNum);
          }}>支持</button>

          <button onClick={() => {
              oppNum++;
              console.log(oppNum);
          }}>反对</button>
      </div>
  </div>;
};
export default Vote;
```

### 类组件

#### 类相关知识点

```javascript
class Parent {
  constructor(x,y) {
    // new的时候，执行的构造函数「可写可不写:需要接受传递进来的实参信息，才需要设置constructor」
    this.total = x + y;
  }
  num = 20; // 等价于this.num=2000给实例, 这是私有属性
  getNum = () => {
    // 箭头函数没有自己的this，所用到的this是宿主环境中的
    console.log(this)
  }
  sum() {
    // 类似于sum=function sum(){}不是箭头函数
    // 它是给Parent.prototype上设置公共的方法「sum函数是不可枚举的」
  }
  // 把构造函数当做一个普通对象，为其设置静态的私有属性方法Parent.xxx
  static avg = 100
  static average() {

  }
}
Parent.prototype.y = 100;
let p = new Parent(10,20);
console.log(p);
p.getNum();
```

![image-20231210203511554](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312102035734.png)



![image-20231210190806565](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202312101908044.png)

#### 类组件继承

基于extends实现继承

1. 首先基于call继承React.Component.call(this) //this->Parent类的实例p

   ```jsx
   function Component(props,context, updater){
       this.props = props;
       this.context = context;
       this.refs = emptyobject;
       this.updater = updater || ReactNoopupdateQueue;
   }
   // 给创建的实例p设置四个私有属性: props/context/ refs/updater
   ```

2. 再基于原型继承`Parent.prototype.__proto__ === React.Component.prototype`

   实例> Parent.prototype -> React.Component.prototype -> 0bject.prototype

   实例除了具备Parent.prototype提供的方法之外，还具备了React.component. prototype原型上提供的方法: 

   `isReactComponent`、 `setState`、`forceUptate`

3. 只要自己设置了constructor，则内部第一句话一定要执行super( )

```jsx
class Parent extends React.Component {
    constructor(props) {
        // this->p   props->获取的属性
        // super() 相当于  React.Component.call(this)
        // this.props = undefined  this.context = undefined  this.refs = {}
        super(props);
        this.props = props  this.context = undefined
    }
    x = 100;
	getX() {
        
    } 
}
let p = new Parent();
console.log(p);
```

#### 类组件细节

**创建类组件**

* 创建一个构造函数(类)
  * 要求必须继承React.Component/PureComponent这个类
  *  我们习惯于使用ES6中的class创建类「因为方便」
  * 必须给当前类设置一个render的方法「放在其原型上」：在render方法中，返回需要渲染的视图

 **从调用类组件「new Vote({...})」开始，类组件内部发生的事情：**

1. 初始化属性 && 规则校验
   先规则校验，校验完毕后，再处理属性的其他操作！！
    方案一： 

   ```javascript
   constructor(props) {
       super(props); //会把传递进来的属性挂载到this实例上
       console.log(this.props); //获取到传递的属性
   }
   ```

   方案二：即便我们自己不再constructor中处理「或者constructor都没写」，在constructor处理完毕后，React内部也会把传递的props挂载到实例上；所以在其他的函数中，只要保证this是实例，就可以基于this.props获取传递的属性！

   同样this.props获取的属性对象也是被冻结的{只读的}  **Object.isFrozen(this.props)->true**

 2. 初始化状态
      状态：后期修改状态，可以触发视图的更新
      需要手动初始化，如果我们没有去做相关的处理，则默认会往实例上挂载一个state，初始值是null => `this.state=null`
      手动处理：
      `state = {
        ...
      };`
      **修改状态，控制视图更新**
      this.state.xxx=xxx ：这种操作仅仅是修改了状态值，但是无法让视图更新
      想让视图更新，我们需要基于React.Component.prototype提供的方法操作：

      * ` this.setState(partialState)` 既可以修改状态，也可以让视图更新 「推荐」

        ```javascript
        // partialState:部分状态
        this.setState({
        	xxx:xxx
        });
        ```

         *  ` this.forceUpdate() `强制更新

   3. 触发 componentWillMount 周期函数(钩子函数)：组件第一次渲染之前
      钩子函数：在程序运行到某个阶段，我们可以基于提供一个处理函数，让开发者在这个阶段做一些自定义的事情

      + 此周期函数，目前是不安全的「虽然可以用，但是未来可能要被移除了，所以不建议使用」
        + 控制会抛出黄色警告「为了不抛出警告，我们可以暂时用 `UNSAFE_componentWillMount`」
      + 如果开启了`React.StrictMode`「React的严格模式」，则我们使用 UNSAFE_componentWillMount 这样的周期函数，控制台会直接抛出红色警告错误！！
        React.StrictMode VS "use strict"
        + "use strict"：JS的严格模式
        + React.StrictMode：React的严格模式，它会去检查React中一些不规范的语法、或者是一些不建议使用的API等！！

4. 触发 render 周期函数：渲染

5. 触发 componentDidMount 周期函数：第一次渲染完毕

      ​	已经把virtualDOM变为真实DOM了「所以我们可以获取真实DOM了」

> Vue中，我们直接修改状态值，视图自己就会更新「原理:我们对状态做了**数据劫持**，我们修改值的时候，会触发set劫持函数，在这个函数中，会通知视图更新」

```jsx
import React from 'react';
import PropTypes from 'prop-types'
class ClassComponent extends React.Component {
  // 属性规则校验
  static defaultProps = {
    num: 0
  };
  static propTypes = {
    title: PropTypes.string.isRequired,
    num: PropTypes.number
  }
  // 初始化状态
  state = {
    supNum: 20,
    oppNum: 10
  };

  render() {
    let {title} = this.props,
    { supNum, oppNum } = this.state;
    console.log("组件渲染")
    return <div className="vote-box">
            <div className="header">
              <h2 className="title">{title}</h2>
              <span>{supNum + oppNum}人</span>
            </div>
            <div className="main">
              <p>支持人数：{supNum}人</p>
              <p>反对人数：{oppNum}人</p>
            </div>
            <div className="footer">
              <button onClick={() => {
                // 修改状态，让视图更新
                this.setState({
                  supNum: supNum + 1
                });
              }}>支持</button>

              <button onClick={() => {
                this.state.oppNum++;
                // 强制让视图更新
                this.forceUpdate();
              }}>反对</button>
            </div>
          </div>;
  }

  UNSAFE_componentWillMount() {
    console.log("组件渲染前")
  }
  componentDidMount() {
    console.log("组件渲染后")
  }
}

export default ClassComponent
```

 render函数在渲染的时候，如果type是：

* 字符串：创建一个标签

* 普通函数：把函数执行，并且把props传递给函数

* 构造函数：把构造函数基于new执行「也就是创建类的一个实例」，也会把解析出来的props传递过去
       + 每调用一次类组件都会创建一个单独的实例
            + 把在类组件中编写的render函数执行，把返回的jsx「virtualDOM」当做组件视图进行渲染！！

    ```jsx
    new Vote({
    	title:'React其实还是很好学的!'
    })    
    ```


#### 组件更新逻辑

![image-20240106161534950](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401061615111.png)

**第一种：组件内部的状态被修改，组件会更新**

1. 触发 `shouldComponentUpdate` 周期函数：是否允许更新

   ```javascript
     shouldComponentUpdate(nextProps, nextState) {
       // nextState:存储要修改的最新状态
       // this.state:存储的还是修改前的状态「此时状态还没有改变」
       console.log("shouldComponentUpdate:", this.state, nextState);
   
       // 此周期函数需要返回true/false
       //   返回true：允许更新，会继续执行下一个操作
       //   返回false：不允许更新，接下来啥都不处理
       return true;
     }
   ```

2. 触发 `componentWillUpdate` 周期函数：更新之前

   此周期函数也是**不安全**的

   在这个阶段，状态/属性还没有被修改

   ```javascript
     UNSAFE_componentWillUpdate(nextProps, nextState) {
       // 此时还没有更改
       console.log('componentWillUpdate:', this.state, nextState);
     }
   ```

3. 修改状态值/属性值「让this.state.xxx改为最新的值」

4. 触发 render 周期函数：组件更新

   * 按照最新的状态/属性，把返回的`JSX`编译为`virtualDOM`

   * 和上一次渲染出来的`virtualDOM`进行对比「**DOM-DIFF**」

   * 把差异的部分进行渲染「渲染为真实的DOM」

5. 触发 `componentDidUpdate` 周期函数：组件更新完毕

   ```javascript
     componentDidUpdate() {
       // 此时数据已经修改
       console.log('componentDidUpdate: 组件更新完毕')
     }
   ```

![image-20240106164539278](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401061645373.png)

> 特殊说明：如果我们是基于` this.forceUpdate()` 强制更新视图，会跳过` shouldComponentUpdate `周期函数的校验，直接从 `WillUpdate` 开始进行更新「也就是：视图一定会触发更新」！
>
> `this.setState()`更新视图，如果` shouldComponentUpdate `周期返回**false**就不会往下走其他的周期函数，而`this.forceUpdate()`会一直走下去
>
> **更新逻辑**：
>
> `this.setState()` ->`shouldComponentUpdate() return true`-> `componentWillUpdate()`->修改状态值/属性值->`render()`->`componentDidUpdate()`

**第二种：父组件更新，触发的子组件更新**

1. 触发 componentWillReceiveProps 周期函数：接收最新属性之前

   周期函数是**不安全**的

   ```javascript
     UNSAFE_componentWillReceiveProps(nextProps) {
       // this.props:存储之前的属性
       // nextProps:传递进来的最新属性值
       console.log("componentWillReceiveProps:", this.props, nextProps)
     }
   ```

2. 触发 `shouldComponentUpdate` 周期函数

3. ....其他逻辑和第一种的相同

![image-20240106164449130](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401061644227.png)

#### 组件销毁逻辑

1. 触发 componentWillUnmount 周期函数：组件销毁之前

   ```javascript
     componentWillUnmount() {
       // 组件销毁前周期函数
       console.log("componentWillUnmount: 组件销毁前周期函数")
     }
   ```

2. 销毁

#### 组件更新销毁完整代码

```javascript
import React from 'react';
import PropTypes from 'prop-types'
class ClassComponent extends React.Component {
  // 属性规则校验
  static defaultProps = {
    num: 0
  };
  static propTypes = {
    title: PropTypes.string.isRequired,
    num: PropTypes.number
  }
  // 初始化状态
  state = {
    supNum: 20,
    oppNum: 10
  };

  render() {
    let {title} = this.props,
    { supNum, oppNum } = this.state;
    console.log("组件渲染")
    return <div className="vote-box">
            <div className="header">
              <h2 className="title">{title}</h2>
              <span>{supNum + oppNum}人</span>
            </div>
            <div className="main">
              <p>支持人数：{supNum}人</p>
              <p>反对人数：{oppNum}人</p>
            </div>
            <div className="footer">
              <button onClick={() => {
                // 修改状态，让视图更新
                this.setState({
                  supNum: supNum + 1
                });
              }}>支持</button>

              <button onClick={() => {
                this.state.oppNum++;
                // 强制让视图更新
                this.forceUpdate();
              }}>反对</button>
            </div>
          </div>;
  }

  UNSAFE_componentWillMount() {
    console.log("组件渲染前")
  }
  componentDidMount() {
    console.log("组件渲染后")
  }

  UNSAFE_componentWillReceiveProps(nextProps) {
    // this.props:存储之前的属性
    // nextProps:传递进来的最新属性值
    console.log("componentWillReceiveProps:", this.props, nextProps)
  }

  shouldComponentUpdate(nextProps, nextState) {
    // nextState:存储要修改的最新状态
    // this.state:存储的还是修改前的状态「此时状态还没有改变」
    console.log("shouldComponentUpdate:", this.state, nextState);

    // 此周期函数需要返回true/false
    //   返回true：允许更新，会继续执行下一个操作
    //   返回false：不允许更新，接下来啥都不处理
    return true;
  }

  UNSAFE_componentWillUpdate(nextProps, nextState) {
    // 此时还没有更改
    console.log('componentWillUpdate:', this.state, nextState);
  }

  componentDidUpdate() {
    // 此时数据已经修改
    console.log('componentDidUpdate: 组件更新完毕')
  }
  componentWillUnmount() {
    // 组件销毁前周期函数
    console.log("componentWillUnmount: 组件销毁前周期函数")
  }
}

export default ClassComponent
```

#### 父子组件嵌套渲染逻辑

  父子组件嵌套，处理机制上遵循深度优先原则：父组件在操作中，遇到子组件，一定是把子组件处理完，父组件才能继续处理
    + 父组件第一次渲染
      父 `willMount` -> 父 `render`「子 `willMount` -> 子 `render` -> 子`didMount`」 -> 父`didMount` 
   + 父组件更新：
  父 `shouldUpdate` -> 父`willUpdate` -> 父 `render` 「子`willReceiveProps` -> 子 `shouldUpdate` -> 子`willUpdate` -> 子 `render` -> 子 `didUpdate`」-> 父 `didUpdate`

* 父组件销毁：
  父 `willUnmount` -> 处理中「子`willUnmount` -> 子销毁」-> 父销毁

### 函数组件和类组件对比

 函数组件是“静态组件”：
   + 组件第一次渲染完毕后，无法基于“内部的某些操作”让组件更新「**无法实现“自更新”**」；但是，如果调用它的父组件更新了，那么相关的子组件也一定会更新「可能传递最新的属性值进来」；
   + 函数组件具备：**属性**...「其他状态等内容几乎没有」
   + 优势：比类组件处理的**机制简单**，这样导致函**数组件渲染速度更快**！！

类组件是“动态组件”：

   + 组件在第一渲染完毕后，除了父组件更新可以触发其更新外，我们还可以通过：**this.setState修改状态** 或者 this.forceUpdate 等方式，让组件**实现“自更新”**！！
   + 类组件具备：**属性**、**状态**、**周期函数**、ref...「几乎组件应该有的东西它都具备」
   + 优势：功能强大！！

 ===>Hooks组件「推荐」：具备了函数组件和类组件的各自优势，在函数组件的基础上，基于hooks函数，让函数组件也可以拥有状态、周期函数等，让函数组件也可以实现自更新「动态化」！！

### 补充

#### PureComponent

>  PureComponent和Component的区别：
>
>    PureComponent会给类组件默认加一个`shouldComponentUpdate`周期函数
>
>    + 在此周期函数中，它对新老的属性/状态 会做一个**浅比较**
>       如果经过浅比较，发现属性和状态并没有改变，则返回false「**也就是不继续更新组建**」；有变化才会去更新！！

浅比较

![image-20240106192756188](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401062134134.png)

```jsx
import React from 'react'

// 检测是否为对象
const isObject = function isObject(obj) {
  return obj !== null && /^(object|function)$/.test(typeof obj);
}
// 对象浅比较的方法
const shallowEqual = function shallowEqual(objA, objB) {
  // 其中一个不是对象直接返回false
  if(!isObject(objA) || !isObject(objB)) return false;
  
  if(objA === objB) return true;

  // 先比较成员的数量
  let keysA = Reflect.ownKeys(objA), 
      keysB = Reflect.ownKeys(objB);
  if(keysA.length !== keysB.length) return false;

  // 数量一直，再逐一比较内部的成员
  for(let i = 0; i < keysA.length; i++) {
    let key = keysA[i];
    // 这里用Object.is是为了让NaN和NaN相等
    // // 如果一个对象中有这个成员，一个对象中没有；或者，都有这个成员，但是成员值不一样；都应该被判定为不相同！！
    if(!objB.hasOwnProperty(key) || ! Object.is(objA[key], objB[key])) {
      return false;
    }
  }
  return true;
}

class Demo extends React.PureComponent{
  state = {
      arr: [10, 20, 30] //0x001
  };
  render() {
    let { arr } = this.state; //arr->0x001
    return <div>
        {arr.map((item, index) => {
            return <span key={index} style={{
                display: 'inline-block',
                width: 100,
                height: 100,
                background: 'pink',
                marginRight: 10
            }}>
                {item}
            </span>;
        })}

        <br />

        <button onClick={() => {
            arr.push(40); //给0x001堆中新增一个40
            /* 
            // 无法更新的
            console.log(this.state.arr); //[10,20,30,40]
            this.setState({ arr }); //最新修改的转态地址，还是0x001「状态地址没有改」 
            */
           //  this.forceUpdate(); //跳过默认加的shouldComponentUpdate，直接更新
            this.setState({
                arr: [...arr] //我们是让arr状态值改为一个新的数组「堆地址」
            })
        }}>新增SPAN</button>
    </div >;
  }

  // PureComponent内部自动加了shouldComponentUpdate生命周期函数
  // shouldComponentUpdate(nextProps, nextState) {
  //   let { props, state } = this;
  //   // props/state：修改之前的属性状态
  //   // nextProps/nextState：将要修改的属性状态
  //   return !shallowEqual(props, nextProps) || !shallowEqual(state, nextState);
  // }
}

export default Demo
```

#### 获取DOM元素

**受控组件**：基于修改数据/状态，让视图更新，达到需要的效果 「推荐」

```javascript
  // 组件渲染后的生命周期函数
  componentDidMount() {
    // 这个时候虚拟DOM已经渲染为真实DOM了，可以直接获取
    console.log(document.querySelector('#dom'));
  }
```

**非受控组件**：基于ref获取DOM元素，我们操作DOM元素，来实现需求和效果「偶尔」

基于ref获取DOM元素的语法

1. 给需要获取的元素设置ref='xxx'，后期基于this.refs.xxx去获取相应的DOM元素「不推荐使用：在React.StrictMode模式下会报错」

   ```jsx
   <h2 className="title" ref="titleBox">温馨提示</h2>
     // 组件渲染后的生命周期函数
     componentDidMount() {
       // ref方式获取元素
       console.log(this.refs.titleBox);
     }
   ```

2. 把ref属性值设置为一个函数

   > `ref={x=>this.xxx=x}`
   >
   > x是函数的形参：存储的就是当前DOM元素
   >
   > 获取的DOM元素“x”直接挂在到实例的属性"xxx"上，获取直接`this.xxx`

   ```jsx
   <h2 className="title" ref = {x => this.box = x}>友情提示</h2>
     // 组件渲染后的生命周期函数
     componentDidMount() {
       console.log(this.box)
     }
   ```

3. 基于React.createRef()方法创建一个REF对象

   > `this.xxx=React.createRef();` 等价于` this.xxx={current:null}`
   > 使用：ref={REF对象(this.xxx)}
   > 获取：this.xxx.current

   ```jsx
   class Demo extends React.Component { 
     box2 = React.createRef(); // 等价于 创建了一个对象 this.box2 = { current: null};
     render() {
       return <div>
           <h2 className="title" ref={this.box2}>郑重提示</h2>
       </div>;
     }
     // 组件渲染后的生命周期函数
     componentDidMount() {
       console.log(this.box2.current)
     }
   }
   ```

原理：在render渲染的时候，会获取virtualDOM的ref属性

* 如果属性值是一个**字符串**，则会给this.refs增加这样的一个成员，成员值就是当前的DOM元素
* 如果属性值是一个**函数**，则会把函数执行，把当前DOM元素传递给这个函数「x->DOM元素」,而在函数执行的内部，我们一般都会把DOM元素直接挂在到实例的某个属性上
*  如果属性值是一个**REF对象**，则会把DOM元素赋值给对象的current属性

#### 函数组件Ref

* 给**元素标签**设置ref，目的：获取对应的DOM元素 

* 给**类组件**设置ref，目的：获取当前调用组件创建的实例「后续可以根据实例获取子组件中的相关信息」

* 给**函数组件**设置ref，直接报错：Function components cannot be given refs. Attempts to access this ref will fail.

  * 但是我们让其配合 `React.forwardRef` 实现ref的转发

  * 目的：获取函数子组件内部的某个元素

```javascript
import React from 'react'

class Child1 extends React.Component {
  emBox = React.createRef()
  state = {
      x: 100,
      y: 200
  };
  render() {
      return <div>
          <span>子组件1</span>
          <em ref={this.emBox}>100</em>
      </div>;
  }
}

const Child2 = React.forwardRef(function Child2(props, ref) {
  return <>
    子组件2
    <button ref={ref}>按钮</button>
  </>
})

class Demo extends React.Component {
  render() {
    return <>
      <h1>1111</h1>
      <Child1 ref={x => this.child1 = x}></Child1>
      <Child2 ref={x => this.child2 = x}></Child2>
    </>
  }

  componentDidMount() {
    console.log(this.child1); // 存储的是:子组件的实例对象
    console.log(this.child2);
  }
}

export default Demo
```

![image-20240106222844163](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401062228354.png)