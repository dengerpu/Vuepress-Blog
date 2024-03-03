title: React全家桶
index_img: /img/article/empty.png
categories:

  - 分类
tags:
  - 标签1
  - 标签2
date: 2023-11-19 16:25:55

[React官网](https://zh-hans.react.dev/learn)

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

#### setState的进阶研究

React18中，对于setState的操作，采用了 `批处理`！

- 构建了队列机制
- 统一更新，提高视图更新的性能
- 处理流程更加稳健

在React 18之前，我们只在 `React合成事件/周期函数`期间批量更新；默认情况下，React中不会对 promise、setTimeout、原生事件处理（native event handlers）或其它React默认不进行批处理的事件进行批处理操作！

源码

```javascript
// state可以是函数（传递函数可以获取先前的值）也可以是集合， callback回调函数
setState<K extends keyof S>(
            state: ((prevState: Readonly<S>, props: Readonly<P>) => Pick<S, K> | S | null) | (Pick<S, K> | S | null),
            callback?: () => void,
        ): void;
```

> `this.setState([partialState],[callback])`
>
> * `partialState` 支持部分状态更改 
>
>   ```jsx
>     this.setState({
>               x:100 //不论总共有多少状态，我们只修改了x，其余的状态不动
>           });
>   ```
>
> * `callback` 在状态更改/视图更新完毕后触发执行「也可以说**只要执行了setState，callback一定会执行**」
>
>   * 发生在`componentDidUpdate`周期函数之**后**「DidUpdate会在任何状态更改后都触发执行；而回调函数方式，可以在指定状态更新后处理一些事情；」
>   * 即便我们基于`shouldComponentUpdate`阻止了状态/视图的更新，DidUpdate周期函数肯定不会执行了，但是我们设置的这个callback回调函数依然会被触发执行！！
>   * 类似于Vue框架中的`$nextTick`！

代码演示

```jsx
import React from 'react'

class Demo extends React.Component {
  state = {
      x: 10,
      y: 5,
      z: 0
  };
  handle = () => {
      let { x, y, z } = this.state;
      this.setState({ x: x + 1 }, () => {
          // 3
        console.log("setState回调函数：当前部分状态更新完毕后执行")
      });
  };
  shouldComponentUpdate() {
      // 1
    return true;
  }
  componentDidUpdate() {
      // 2
    console.log("componentDidUpdate,更新完毕后执行，在setState回调函数之前")
  }
  render() {
      console.log('视图渲染:RENDER');
      let { x, y, z } = this.state;
      return <div>
          x:{x} - y:{y} - z:{z}
          <br />
          <button onClick={this.handle}>按钮</button>
      </div>;
  }
}
export default Demo;
```

事件更新机制

```javascript
      // 同时修改三个状态值，只会出发一次视图更新
      this.setState({ 
        x: x + 1,
        y: y + 1,
        z: z + 1
      });
```

> 在React18中，`setState`操作都是**异步**的「不论是在哪执行，例如：合成事件、周期函数、定时器...」
>
> 目的：实现状态的批处理「统一处理」
>
> * 有效减少更新次数，降低性能消耗
> * 有效管理代码执行的逻辑顺序
>
>
> 原理：利用了更新队列「updater」机制来处理的
>    + 在当前相同的时间段内「浏览器此时可以处理的事情中」，遇到setState会立即放入到更新队列中！
>          此时状态/视图还未更新
>    + 当所有的代码操作结束，会“刷新队列”「通知更新队列中的任务执行」：把所有放入的setState合并在一起执行，只触发一次视图更新「批处理操作」

![image-20240107203514983](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401072035292.png)

![image-20240107204249513](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401072042679.png)

>在React18 和 React16中，关于setState是同步还是异步，是有一些区别的！
>
>* React18中：不论在什么地方执行setState，它**都是异步**的「都是基于updater更新队列机制，实现的批处理」
>* React16中：如果在**合成事件**「jsx元素中基于onXxx绑定的事件」、**周期函数**中，setState的操作是**异步**的！！但是如果setState出现在**其他异步操作中**「例如：定时器、手动获取DOM元素做的事件绑定等」，它将变为**同步**的操作「立即更新状态和让视图渲染」！！

##### `flushSync`

> flushSync:可以刷新“updater更新队列”，也就是让修改状态的任务立即批处理一次！！

```jsx
import React from 'react'
import { flushSync } from 'react-dom'
// flushSync:可以刷新“updater更新队列”，也就是让修改状态的任务立即批处理一次！！
class Demo extends React.Component {
  state = {
      x: 10,
      y: 5,
      z: 0
  };

  handle = () => {
      let { x, y, z } = this.state;
      this.setState({ x: x + 1 })
      console.log(this.state); //10/5/0
      flushSync(() => {
        this.setState({ z: z + 1 });
        console.log(this.state); //10/5/0
      }) 
      console.log(this.state) //11/6/0
      // flushSync();可以这样直接调用
      
      // 在修改z之前，要保证x/y都已经更改和让视图更新了
      this.setState({ z: this.state.x + this.state.y });
  };
  shouldComponentUpdate() {
    return true;
  }
  componentDidUpdate() {
    console.log("componentDidUpdate,更新完毕后执行，在setState回调函数之前")
  }
	.....
}

export default Demo;
```

![image-20240107204513938](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401072045124.png)

##### setState传递函数

```jsx
import React from "react";
class Demo extends React.Component {
    state = {
        x: 0
    };

    handle = () => {
        for (let i = 0; i < 20; i++) {
             this.setState({
                x: this.state.x + 1
            }); 
        }
        console.log(this.state) // 这个时候打印this.state.x 还是为0
        // 这样最后循环20次，只会触发一次render渲染,并且x渲染在页面上的值为1，因为循环20次都没改
    };
	......
}

export default Demo;
```

> 
> setState接收的参数还可以是一个函数，在这个函数中可以拿先前的状态，并通过这个函数的返回值得到下一个状态
> ```jsx
>  this.setState((prevState)=>{
>     // prevState:存储之前的状态值
>     // return的对象，就是我们想要修改的新状态值「支持修改部分状态」
>     return {
>         xxx:xxx
>     };
>  })
> ```

想让最后变为20

```jsx
import React from "react";
import { flushSync } from 'react-dom';

/* 
 this.setState((prevState)=>{
    // prevState:存储之前的状态值
    // return的对象，就是我们想要修改的新状态值「支持修改部分状态」
    return {
        xxx:xxx
    };
 })
 */
class Demo extends React.Component {
    state = {
        x: 0
    };
    handle = () => {
        for (let i = 0; i < 20; i++) {
          this.setState((prevState) => {
            return {
              x: prevState.x + 1
            }
          })
        }
      };
	.....
}

export default Demo;
```

更新流程：![image-20240107205931538](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401072059740.png)

## 合成事件

> `Synthetic`
> 合成事件是围绕浏览器原生事件，充当跨浏览器包装器的对象；它们将不同浏览器的行为合并为一个 API，这样做是为了确保事件在不同浏览器中显示一致的属性！

### 合成事件的基本操作

在JSX元素上，直接基于 `onXxx={函数}` 进行事件绑定！
浏览器标准事件，在React中大部分都支持
[https://developer.mozilla.org/zh-CN/docs/Web/Events#%E6%A0%87%E5%87%86%E4%BA%8B%E4%BB%B6](https://developer.mozilla.org/zh-CN/docs/Web/Events#标准事件)

```jsx
import React, { Component } from "react";
export default class App extends Component {
    state = {
        num: 0
    };
    render() {
        let { num } = this.state;
        return <div>
            {num}
            <br />
            <button onClick={(ev) => {
                // 合成事件对象 ：SyntheticBaseEvent
                console.log(ev); 
            }}>处理</button>
        </div>;
    }
};
```

#### 合成事件中的this和传参处理

在类组件中，我们要时刻保证，合成事件绑定的函数中，里面的this是当前类的实例！

```jsx
import React from "react";

class Demo extends React.Component {
    
    handle1() { //Demo.prototype => Demo.prototype.handle=function handle(){}
        console.log(this); //undefined
    }
    handle2(x, y, ev) {
        // 只要方法经过bind处理了，那么最后一个实参，就是传递的合成事件对象！！
        console.log(this, x, y, ev); //实例 10 20 合成事件对象
    }
    handle3 = (ev) => {  //实例.handle3=()=>{....}
        console.log(this); //实例
        console.log(ev); //SyntheticBaseEvent 合成事件对象「React内部经过特殊处理，把各个浏览器的事件对象统一化后，构建的一个事件对象」
    };
    handle4 = (x, ev) => {
        console.log(x, ev); //10 合成事件对象
    };

    render() {
        return <div>
            <button onClick={this.handle1}>按钮1</button>
            <button onClick={this.handle2.bind(this, 10, 20)}>按钮2</button>
            <button onClick={this.handle3}>按钮3</button>
            <button onClick={this.handle4.bind(null, 10)}>按钮4</button>
        </div>;
    }
}

export default Demo;
```

> 基于React内部的处理，如果我们给**合成事件绑定一个“普通函数”**，当事件行为触发，绑定的函数执行；方法中的**this会是undefined**「不好」！！ 解决方案：this->实例:
>
> * 我们可以基于JS中的**bind**方法：预先处理函数中的this和实参的。（**apply和call会执行**）
> * 推荐：当然也可以把绑定的函数设置为“**箭头函数**”，让其使用上下文中的this
>
> bind在React事件绑定的中运用
>
> * 绑定的方法是一个普通函数，需要改变函数中的this是实例，此时需要用到bind「一般都是绑定箭头函数」
> * 想给函数传递指定的实参，可以基于bind预先处理「bind会把事件对象以最后一个实参传递给函数」 

#### 合成事件对象

合成事件对象`SyntheticBaseEvent`：我们在React合成事件触发的时候，也可以获取到事件对象，只不过此对象是合成事件对象「React内部经过特殊处理，把各个浏览器的事件对象统一化后，构建的一个事件对象」

 合成事件对象中，也包含了浏览器内置事件对象中的一些属性和方法「常用的基本都有」

   + clientX/clientY
   + pageX/pageY
   + target
   + type
   + preventDefault
   + stopPropagation
   + `nativeEvent`：基于这个属性，可以获取浏览器内置『原生』的事件对象

![image-20240108202146579](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401082021723.png)

### 事件委托

事件和事件绑定

- 事件是浏览器内置行为
- 事件绑定是给事件行为绑定方法
  - 元素.onxxx=function…
  - 元素.addEventListener(‘xxx’,function(){},true/false)

事件的传播机制

- 捕获 CAPTURING_PHASE
- 目标 AT_TARGET
- 冒泡 BUBBLING_PHASE

阻止冒泡传播

- ev.cancelBubble=true 「<=IE8」
- ev.stopPropagation()
- ev.stopImmediatePropagation() 阻止监听同一事件的其他事件监听器被调用

> `el.addEventListener(type, listener, [useCapture])`
>
> * `type`：事件类型，click、mouseenter 等
>
> * `listener`：事件处理函数，事件发⽣时，就会触发该函数运⾏。
>
> * `useCapture`：布尔值，规定**是否是捕获型**，默认为 false（冒泡）。因为是可选的，往往也会省略它。

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件委托</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        html,
        body {
            height: 100%;
            overflow: hidden;
        }

        .center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

        #root {
            width: 300px;
            height: 300px;
            background: lightblue;
        }

        #outer {
            width: 200px;
            height: 200px;
            background: lightgreen;
        }

        #inner {
            width: 100px;
            height: 100px;
            background: lightcoral;
        }
    </style>
</head>

<body>
    <div id="root" class="center">
        <div id="outer" class="center">
            <div id="inner" class="center"></div>
        </div>
    </div>

    <!-- IMPORT JS -->
    <script>
        // 层级结构 window -> document -> html -> body -> root -> outer -> inner
        // ev.stopPropagation：阻止事件的传播「包含捕获和冒泡」
        // ev.stopImmediatePropagation：也是阻止事件传播，只不过它可以把当前元素绑定的其他方法「同级的」，如果还未执行，也不会让其再执行了！！
        const html = document.documentElement,
            body = document.body,
            root = document.querySelector('#root'),
            outer = document.querySelector('#outer'),
            inner = document.querySelector('#inner');

        root.addEventListener('click', function (ev) {
            console.log('root 捕获');
        }, true);
        root.addEventListener('click', function () {
            console.log('root 冒泡');
        }, false);

        outer.addEventListener('click', function () {
            console.log('outer 捕获');
        }, true);
        outer.addEventListener('click', function () {
            console.log('outer 冒泡');
        }, false);

        inner.addEventListener('click', function () {
            console.log('inner 捕获');
        }, true);
        inner.addEventListener('click', function (ev) {
            ev.stopImmediatePropagation(); // 给这个元素绑定的同样事件不会再执行了
            // ev.stopPropagation();
            console.log('inner 冒泡1');
        }, false);  
        inner.addEventListener('click', function (ev) {
            console.log('inner 冒泡2');
        }, false);
    </script>
</body>

</html>
```

![image-20240107214527923](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401072145088.png)

事件委托

```javascript
  <script>
        /* 
        事件委托：利用事件的传播机制，实现的一套事件绑定处理方案 
          例如：一个容器中，有很多元素都要在点击的时候做一些事情
            传统方案：首先获取需要操作的元素，然后逐一做事件绑定
            事件委托：只需要给容器做一个事件绑定「点击内部的任何元素，根据事件的冒泡传播机制，都会让容器的点击事件也触发；我们在这里，根据事件源，做不同的事情就可以了；」
          优势：
            + 提高JS代码运行的性能，并且把处理的逻辑都集中在一起！！
            + 某些需求必须基于事件委托处理，例如：除了点击xxx外，点击其余的任何东西，都咋咋咋...
            + 给动态绑定的元素做事件绑定
            + ...
          限制：
            + 当前操作的事件必须支持冒泡传播机制才可以
              例如：mouseenter/mouseleave等事件是没有冒泡传播机制的
            + 如果单独做的事件绑定中，做了事件传播机制的阻止，那么事件委托中的操作也不会生效！！
        */
        const body = document.body;
        body.addEventListener('click', function (ev) {
            // ev.target:事件源「点击的是谁，谁就是事件源」
            let target = ev.target,
                id = target.id;
            if (id === "root") {
                console.log('root');
                return;
            }
            if (id === "inner") {
                console.log('inner');
                return;
            }
            if (id === "AAA") {
                console.log('AAA');
                return;
            }
            // 如果以上都不是，我们处理啥....
        });
        const outer = document.querySelector('#outer');
        outer.addEventListener('click', function (ev) {
            console.log('outer');
            ev.stopPropagation();
        });

        /* const body = document.body,
            root = document.querySelector('#root'),
            outer = document.querySelector('#outer'),
            inner = document.querySelector('#inner');
        root.addEventListener('click', function () {
            console.log('root');
        });
        outer.addEventListener('click', function () {
            console.log('outer');
        });
        inner.addEventListener('click', function () {
            console.log('inner');
        }); */
    </script>
```

> **`事件委托(代理)，就是利用事件的“冒泡传播机制”实现的`**
>
> 例如：给父容器做统一的事件绑定（点击事件），这样点击容器中的任意元素，都会传播到父容器上，触发绑定的方法！在方法中，基于不同的事件源做不同的事情！
>
> - 性能得到很好的提高「减少内存消耗」
> - 可以给动态增加的元素做事件绑定
> - 某些需求必须基于其完成

### 合成事件的底层机制

> 总原则：基于事件委托实现
>
>  React中合成事件的处理原理
>
>  “绝对不是”给当前元素基于addEventListener单独做的事件绑定，React中的合成事件，都是**基于“事件委托”**处理的！
>
> * 在React17及以后版本，都是**委托给#root**这个容器「**捕获和冒泡都做了委托**」；
> * 在17版本以前，都是为委托给**document容器**的「而且只做了**冒泡阶段的委托**」；
> * 对于没有实现事件传播机制的事件，才是单独做的事件绑定「例如：onMouseEnter/onMouseLeave...」
>
> 在组件渲染的时候，如果发现JSX元素属性中有 onXxx/onXxxCapture 这样的属性，**不会给当前元素直接做事件绑定**，只是把**绑定的方法赋值给元素的相关属性**！！例如：
>      outer.onClick=() => {console.log('outer 冒泡「合成」');}  //这不是DOM0级事件绑定「这样才是outer.onclick」
>      outer.onClickCapture=() => {console.log('outer 捕获「合成」');}
>      inner.onClick=() => {console.log('inner 冒泡「合成」');}
>      inner.onClickCapture=() => {console.log('inner 捕获「合成」');}
>
> 然后对#root这个容器做了事件绑定「捕获和冒泡都做了」
>
> ​     原因：因为组件中所渲染的内容，最后都会插入到#root容器中，这样点击页面中任何一个元素，最后都会把#root的点击行为触发！！
> ​     而在给#root绑定的方法中，把之前给元素设置的onXxx/onXxxCapture属性，在相应的阶段执行!!

#### React18合成事件原理 

```jsx
const outer = document.querySelector('.outer'),
    inner = document.querySelector('.inner');
/* 原理 */
const dispatchEvent = function dispatchEvent(ev) {
    let path = ev.path,
        target = ev.target;
    [...path].reverse().forEach(elem => {
        let handler = elem.onClickCapture;
        if (typeof handler === "function") handler(ev);
    });
    path.forEach(elem => {
        let handler = elem.onClick;
        if (typeof handler === "function") handler(ev);
    });
};
document.addEventListener('click', function (ev) {
    dispatchEvent(ev);
}, false);
```

![image-20240110161412324](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401101614148.png)

![image-20240110161551610](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401101615962.png)

#### React16合成事件原理

React17以前，是**委托给document元素**，并且**没有实现捕获阶段**的派发

```jsx
const outer = document.querySelector('.outer'),
    inner = document.querySelector('.inner');
/* 原理 */
const dispatchEvent = function dispatchEvent(ev) {
    let path = ev.path,
        target = ev.target;
    [...path].reverse().forEach(elem => {
        let handler = elem.onClickCapture;
        if (typeof handler === "function") handler(ev);
    });
    path.forEach(elem => {
        let handler = elem.onClick;
        if (typeof handler === "function") handler(ev);
    });
};
document.addEventListener('click', function (ev) {
    dispatchEvent(ev);
}, false);

......
```

![合成事件处理原理](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401101642865.png)

![合成事件处理原理2](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401101643017.jpg)

#### 事件对象池

16版本中，存在事件对象池

- 缓存和共享：对于那些被频繁使用的对象，在使用完后，不立即将它们释放，而是将它们缓存起来，以供后续的应用程序重复使用，从而减少创建对象和释放对象的次数，进而改善应用程序的性能！
- 使用完成之后，释放对象「每一项内容都清空」，缓存进去！
- 调用 event.persist() 可以保留住这些值！

17版本及以后，移除了事件对象池！

```javascript
syntheticInnerBubble = (syntheticEvent) => {
    // syntheticEvent.persist();    
    setTimeout(() => {
        console.log(syntheticEvent); //每一项都置为空
    }, 1000);
};
```

![image-20240108163252218](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401081632324.png)

### click延迟和Vue中的事件处理机制

click事件在移动端存在`300ms`延迟

- pc端的click是点击事件
- 移动端的click是单击事件

> 连着点击两下:
>
> * PC端会触发︰两次click、一次dblclick
>
> * 移动端:不会触发click，只会触发dblclick
>
> 单击事件:第一次点击后，监测300ms，看是否有第二次点击操作，如果没有就是单击，如果有就是双击

#### 解决移动端300ms延迟

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

// 解决移动端300ms延迟
import fastclick from 'fastclick';
fastclick.attach(document.body);

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <App />
);
```

也可以自己基于touch事件模型去实现

```jsx
const box = document.querySelector('.box');
box.ontouchstart = function (ev) {
    let point = ev.changedTouches[0];
    this.startX = point.pageX;
    this.startY = point.pageY;
    this.isMove = false;
};
box.ontouchmove = function (ev) {
    let point = ev.changedTouches[0];
    let changeX = point.pageX - this.startX;
    let changeY = point.pageY - this.startY;
    if (Math.abs(changeX) > 10 || Math.abs(changeY) > 10) {
        this.isMove = true;
    }
};
box.ontouchend = function (ev) {
    if (!this.isMove) {
        console.log('点击触发');
    }
};
```

### 循环给元素绑定事件

```jsx
import React from 'react'
class Demo extends React.Component {
  state = {
    arr: [{
        id: 1,
        title: '新闻'
    }, {
        id: 2,
        title: '体育'
    }, {
        id: 3,
        title: '电影'
    }]
  };

  handle = (item) => {
    // item:点击这一项的数据
    console.log('我点击的是：' + item.title);
  }

  render() {
    let { arr } = this.state
    return <>
      {arr.map(item => {
        let {id, title} = item;
        return <span key={id}style={{
            padding: '5px 15px',
            marginRight: 10,
            border: '1px solid #DDD',
            cursor: 'pointer'
        }} onClick={this.handle.bind(this, item)}>
          {title}  
        </span>
      })}
    </>
  }
}

export default Demo;
```

> 按照常理来讲,此类需求用事件委托处理是组好的!!!
>
> 但是在React中，我们循环给元素绑定的合成事件，本身就是**基于事件委托处理**的!!所以无需我们自己再单独的设置事件委托的处理机制!!!

#### Vue中的事件处理机制

> 核心：给创建的DOM元素，单独基于`addEventListener`实现事件绑定
> Vue事件优化技巧：手动基于事件委托处理

```vue
<template>
  <div id="app">
    <ul class="box" @click="handler">
      <li
        class="item"
        v-for="(item, index) in arr"
        :data-item="item"
        :key="index"
      >
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      arr: [10, 20, 30],
    };
  },
  methods: {
    handler(ev) {
      let target = ev.target;
      if (target.tagName === "LI") {
        console.log(target.getAttribute("data-item"));
      }
    },
  },
};
</script>
```

## TASK-TODO项目

### 初始化项目

```shell
create-react-app task-todo
```

#### 暴露配置项

```shell
npm run eject
```

> 该操作是不可逆的，一旦执行了就不能再恢复了
>
> 已修改的文件要先提交git到本地，否则会报错

#### 配置跨域

* 安装

  ```shell
  npm install http-proxy-middleware
  ```

* 在src目录中，新建setupProxy.js

  ```js
  const { createProxyMiddleware } = require("http-proxy-middleware");
  
  module.exports = function (app) {
    app.use(
        createProxyMiddleware("/api", {
            target: "http://127.0.0.1:9000",
            changeOrigin: true,
            ws: true,
            pathRewrite: { "^/api": "" }
        })
    );
  };
  ```

  > 直接在src编写setupProxy.js，其他配置React已经写好了
  >
  > ![image-20240111202854046](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401112028218.png)

#### Ant Design配置

* 安装

  ```shell
  npm install antd --save
  # 使用icons图标要安装下面的
  npm install @ant-design/icons --save
  ```

* 配置国际化

  index.jsx

  ```jsx
  import React from 'react';
  import ReactDOM from 'react-dom/client';
  import zhCN from 'antd/locale/zh_CN'
  import { ConfigProvider } from 'antd';
  import Task from './views/Task';
  
  
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(
    <ConfigProvider locale={zhCN}>
      <Task></Task>
    </ConfigProvider>
  );
  ```

#### 清除默认样式

src/assets/css/reset.min.css

```css
body,h1,h2,h3,h4,h5,h6,hr,p,blockquote,dl,dt,dd,ul,ol,li,button,input,textarea,th,td{margin:0;padding:0}body{font-size:12px;font-style:normal;font-family:"\5FAE\8F6F\96C5\9ED1",Helvetica,sans-serif}small{font-size:12px}h1{font-size:18px}h2{font-size:16px}h3{font-size:14px}h4,h5,h6{font-size:100%}ul,ol{list-style:none}a{text-decoration:none;background-color:transparent}a:hover,a:active{outline-width:0;text-decoration:none}table{border-collapse:collapse;border-spacing:0}hr{border:0;height:1px}img{border-style:none}img:not([src]){display:none}svg:not(:root){overflow:hidden}html{-webkit-touch-callout:none;-webkit-text-size-adjust:100%}input,textarea,button,a{-webkit-tap-highlight-color:rgba(0,0,0,0)}article,aside,details,figcaption,figure,footer,header,main,menu,nav,section,summary{display:block}audio,canvas,progress,video{display:inline-block}audio:not([controls]),video:not([controls]){display:none;height:0}progress{vertical-align:baseline}mark{background-color:#ff0;color:#000}sub,sup{position:relative;font-size:75%;line-height:0;vertical-align:baseline}sub{bottom:-0.25em}sup{top:-0.5em}button,input,select,textarea{font-size:100%;outline:0}button,input{overflow:visible}button,select{text-transform:none}textarea{overflow:auto}button,html [type="button"],[type="reset"],[type="submit"]{-webkit-appearance:button}button::-moz-focus-inner,[type="button"]::-moz-focus-inner,[type="reset"]::-moz-focus-inner,[type="submit"]::-moz-focus-inner{border-style:none;padding:0}button:-moz-focusring,[type="button"]:-moz-focusring,[type="reset"]:-moz-focusring,[type="submit"]:-moz-focusring{outline:1px dotted ButtonText}[type="checkbox"],[type="radio"]{box-sizing:border-box;padding:0}[type="number"]::-webkit-inner-spin-button,[type="number"]::-webkit-outer-spin-button{height:auto}[type="search"]{-webkit-appearance:textfield;outline-offset:-2px}[type="search"]::-webkit-search-cancel-button,[type="search"]::-webkit-search-decoration{-webkit-appearance:none}::-webkit-input-placeholder{color:inherit;opacity:.54}::-webkit-file-upload-button{-webkit-appearance:button;font:inherit}.clearfix:after{display:block;height:0;content:"";clear:both}
```

src/index.css

```css
@import './assets/css/reset.min.css';
```

src/index.js

```js
import './index.css'
```

#### 封装请求

src/api/http.js

```js
import axios from 'axios';
import qs from 'qs';
import { message } from 'antd';
import _ from '../assets/utils/util';


const http = axios.create({
  baseURL: '/api',
  timeout: 5000
})

http.defaults.transformRequest = data => {
  // 转为urlencoded格式字符串
  if(_.isPlainObject(data)) data = qs.stringify(data);
  return data;
}

http.interceptors.response.use(response => {
  return response.data
}, err => {
  // 请求失败
  message.error(err.message);
  return Promise.reject(err)
})

export default http
```

> 基于post请求向服务器发送请求，需要基于请求主体把信息传递给服务器
>
> * 普通对象→变为"[object Object]”字符串传递给服务器「不对的J
> * Axios库对其做了处理，我们写的是普通对象，Axios内部会默认把其变为JSON字符串，传递给服务器!!
>
> 格式要求:
>
> * 字符串
>
>   * json字符串 **application/json** `{"x":10,"name":"张三"}`
>   * urlencoded格式字符串**application/x-www-urlencoded** `"x=10&name=张三"`
>   * 普通字符串text/plain
>
> * FormData对象「用于文件上传」**multipart/form-data**
>
>   * ```js
>     let fm=new FormData();
>     fm.append('file',file);
>     ```
>
>   * buffer或者进制格式

### 编写代码

接口

src/api/index.js

```js
import htpp from './http'

// 获取指定状态的任务信息
export const getTaskList = (state = 0, current = 1, pageSize = 2) => {
  return htpp.get('/getTaskList', {
    params: {
      state,
      page: current,
      limit: pageSize
    }
  })
}

// 新增任务
export const addTask = (task, time) => {
  return htpp.post('/addTask', {
    task,
    time
  })
}

// 删除任务
export const removeTask = (id) => {
  return htpp.get('/removeTask', {
    params: {
      id
    }
  })
}

// 完成任务
export const completeTask = (id) => {
  return htpp.get('/completeTask', {
    params: {
      id
    }
  })
}

```

页面代码：

src/views/Task.jsx

```jsx
import React from 'react'
import { flushSync } from 'react-dom'
import { Button, Tag, Table, Popconfirm, Modal, Form, Input, DatePicker, message } from 'antd';
import '../assets/css/task.scss'
import { getTaskList, addTask, removeTask, completeTask } from '../api';


const zero = function zero(text) {
  text = String(text);
  return text.length < 2 ? '0' + text : text;
}
const formatTime = function formatTime(time) {
  let arr = time.match(/\d+/g);
  let [, month, day = '00', hours, minutes = '00'] = arr;
  return `${zero(month)}-${zero(day)} ${zero(hours)}:${zero(minutes)}`;
}

class Task extends React.Component {
  /* 表格列的数据 */
  columns = [
    {
      title: '编号',
      dataIndex: 'id',
      align: 'center',
      width: '8%'
    },
    {
      title: '任务描述',
      dataIndex: 'task',
      ellipsis: true,
      width: '50%'
    },
    {
      title: '状态',
      dataIndex: 'state',
      align: 'center',
      width: '10%',
      render: text => +text === 1 ? <span style={{color: 'red'}}>未完成</span> : <span style={{color: 'green'}}>已完成</span>
    },
    {
      title: '完成时间',
      dataIndex: 'time',
      align: 'center',
      width: '15%',
      render: (_, record) => {
        let { state, time, complete } = record;
        if (+state === 2) time = complete;
        return formatTime(time)
      }
    }, 
    {
      title: '操作',
      render: (_, record) => {
        let { id, state } = record;
        return <>
          <Popconfirm
            title="您确定要删除此任务吗"
            onConfirm={this.removeTask.bind(this, id)}
          >
            <Button type="link">删除</Button>
          </Popconfirm>
          
          {
            +state !== 2 ? <Popconfirm title="您确定要完成此任务吗" onConfirm={this.updateTaskState.bind(null, id)}>
              <Button type="link">完成</Button>
            </Popconfirm> : null
          }
        </>
      }
    }
  ]
  state = {
    tableData: [], // 表格数据
    modalVisible: false,  // 弹窗是否显示
    saveTaskconfirmLoading: false, // 提交任务loading
    tableLoading: false, // 表格loading
    selectIndex: 0,
    pageInfo: {
      current: 1, // 当前页数
      pageSize: 2, // 每页条数
      showSizeChanger: true, // 显示分页切换器
      showQuickJumper: true, // 显示快速跳转至某页
      pageSizeOptions: [1,2,5,10],
      total: 0,
      showTotal: (total, range) => `${range[0]}-${range[1]} 共 ${total} 条`,
      onChange: (page, pageSize) => this.pageChange(page, pageSize)
    }
  }

  // 页码切换事件
  pageChange = (page, pageSize) => {
    console.log('页码发生变化')
    flushSync(() => {
      this.setState({ pageInfo: { ...this.state.pageInfo, current: page, pageSize } });
    })
    // console.log('页码发生变化')
    this.queryData()
  }

  // 完整状态切换
  changeIndex = (index) => {
    if(this.state.selectIndex === index) return
    
    // this.setState({selectIndex: index}) 
    // 直接这样 this.state.selectIndex还是原来的值，并未及时改变
    // 如果在这个时候发送请求，获取到的selectIndex还是上一次的值

    // 解决方法1
    // this.setState({selectIndex: index}, () => {
    //   // 发送请求获取数据
    //   console.log(this.state.selectIndex)
    // })

    // 解决方法2
    flushSync(() => {
      this.setState({selectIndex: index})
    });
    // 重置当前页的参数
    flushSync(() => {
      this.setState({ pageInfo: { ...this.state.pageInfo, current: 1 } });
    })
    this.queryData();
  }

  // 获取任务列表
  queryData = async () => {
    let {selectIndex} = this.state;
    this.setState({tableLoading: true})
    try {
      let { code, list, page, total } = await getTaskList(selectIndex, this.state.pageInfo.current, this.state.pageInfo.pageSize);
      if(+code !== 0) { // 0代表获取成功
        list = []
      }
      console.log('请求获取到的数据', list)
      this.setState({
        tableData:list,
        pageInfo: {
          ...this.state.pageInfo,
          current: +page,
          total: +total
        }
      })
    } catch (error) {
      message.error('获取任务列表失败')
    }
    this.setState({tableLoading: false})
  }

  // 删除任务
  removeTask = async (id) => {
    let { code } = await removeTask(id);
    if(+code !== 0) {
      message.error('删除任务失败')
      return
    } else {
      this.queryData()
      message.success('删除任务成功')
    } 
  }

  // 修改任务状态
  updateTaskState = async (id) => {
    let {code} = await completeTask(id);
    if(+code !== 0) {
      message.error('修改任务状态失败')
      return
    } else {
      this.queryData()
      message.success('修改任务状态成功')
    } 
  }

  // 提交任务
  saveTask = async () => {
    try {
      // 表单校验
      await this.formRef.validateFields()
      let {task , time} = this.formRef.getFieldsValue();
      time = time.format('YYYY-MM-DD HH:mm:ss');
      this.setState({saveTaskconfirmLoading: true})
      // 向服务器端发送请求
      let { code } = await addTask(task, time);
      if(+code !== 0) {
        message.error('添加任务失败');
      } else {
        // 关闭弹框
        this.closeMode();
        // 获取最新的数据
        this.queryData();
        message.success('添加任务成功');
      }
    }catch (_) {
      message.error('请填写完整信息');
    }
    this.setState({saveTaskconfirmLoading: false})
  }

  // 关闭弹框事件
  closeMode = () => {
    this.setState({
      modalVisible: false,
      saveTaskconfirmLoading: false
    })
    this.formRef.resetFields();
  }

  componentDidMount() {
    this.queryData();
    console.log('获取到的表格数据', this.state.tableData)
  }

  render() {
    console.log('视图更新')
    let {tableData, modalVisible, saveTaskconfirmLoading, selectIndex, tableLoading, pageInfo} = this.state
    console.log('分页器的配置', pageInfo)
    return <div className='task_box'>
      <div className="task_header">
        <h1>TASK OA任务管理系统</h1>
        <Button type="primary" onClick={() => {
          this.setState({
            modalVisible: true
          })
        }}>新增任务</Button>
      </div>
      <div className='task_content'>
        <div className='task_content_header'>
          {['全部', '未完成', '已完成'].map( (item, index) => {
            return <Tag color={selectIndex === index ? '#1677ff' : ''} key={index} onClick={this.changeIndex.bind(null, index)}>{item}</Tag>
          })}
        </div>
        <div className='task_content_table'>
          <Table dataSource={tableData} loading={tableLoading} columns={this.columns}  rowKey="id" pagination={pageInfo}/>
        </div>
      </div>
      {/* 新增任务弹出框 */}
      <Modal title="新增任务窗口" open={modalVisible} maskClosable={false} okText="提交信息" onCancel={this.closeMode} onOk={this.saveTask} confirmLoading={saveTaskconfirmLoading}>
        <Form ref={x => this.formRef = x} layout="vertical" initialValues={{ task: '', time: '' }} validateTrigger="onBlur">
          <Form.Item label="任务描述" name="task" rules={[
            { required: true, message: '任务描述是必填项' }, 
            { min: 6, message: '输入的内容至少6位及以上' }
          ]}>
            <Input.TextArea rows={4} />
          </Form.Item>
          <Form.Item label="任务预期完成时间" name="time" rules={[
            { required: true, message: '预期完成时间是必填项' }
          ]}>
            <DatePicker showTime />
          </Form.Item>
        </Form>
      </Modal>
    </div>
  }
}

export default Task;
```

![image-20240114151901040](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401141519242.png)

最终实现效果：

![image-20240113161938578](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401131619726.png)

### 双向绑定

vue中是实现了双向绑定，但是React中却不是，需要我们自己实现

>  Vue是MVVM框架：数据驱动视图渲染、视图驱动数据更改「自动监测页面中表单元素的变化，从而修改对应的状态」 双向驱动
>  React是MVC框架：数据驱动视图渲染  单向驱动
>
>  需要自己手动实现，视图变化，去修改相关的状态

```jsx
import React from 'react'

class Demo extends React.Component {
  state = {
    email: ''
  };

  render() {
      return <div>
          <input type="text" value={this.state.email} onChange={(ev) => {
            this.setState({
              email: ev.target.value.trim()
            })
            console.log(this.state.email)
          }}/>
      </div>;
  }
}

export default Demo
```

### 总结

#### 细节注意

再修改state对象里面的对象时，要**注意我们只是修改了部分值**，而不是全不值，所有我们还要把之前的赋值过来

```jsx
      this.setState({
        tableData:list,
        pageInfo: {
          ...this.state.pageInfo,
          current: +page,
          total: +total
        }
      })
```

由于setState是异步操作，统一处理，所以视图会存在更新不及时的情况

```jsx
    // this.setState({selectIndex: index}) 
    // 直接这样 this.state.selectIndex还是原来的值，并未及时改变
    // 如果在这个时候发送请求，获取到的selectIndex还是上一次的值

    // 解决方法1
    // this.setState({selectIndex: index}, () => {
    //   // 发送请求获取数据
    //   console.log(this.state.selectIndex)
    // })

    // 解决方法2
    flushSync(() => {
      this.setState({selectIndex: index})
    });
```

数据请求放`componentDidMount`周期函数中

> 正常情况下，我们应该在第一次渲染之前componentWillMount开发发送异步的数据请求
>
> * 请求发送后，不需要等到
> * 继续渲染
> * 在第一次渲染结束后，可能数据已经回来了「即便没回来，也快了J
> * 等到数据获取后，我们修改状态，让视图更新，呈现真实的数据即可
>
> 但是**componentWillMount是不安全的**

#### pm2服务持久化管理

安装

```shell
npm i pm2-g #mac加sudo
```

启动命令

```shell
pm2 start server.js --name TASK
```

重启命令

```shell
pm2 restart TASK
```

停止命令

```shell
pm2 stop TASK
```

删除命令

```shell
pm2 delete TASK
```

> 终端关掉，服务器也在，如果电脑重启，服务器会消失

#### UI组件库

* React的UI组件库
  * PC端:	Anid、 AntdPro....
  * 移动端:	AntdMobile...
* Vue的UI组件库
  * PC端:	element-ui、antd for vue、iview....
  * 移动端:	vant、cube...

> antd组件库自袱按需导入
> 我们安装整个antd，后期在项目中用到哪些组件，最后打包的时候，只打包用的

#### Antd中Form表单处理机制

![image-20240111150138457](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401111501654.png)

#### 时间处理插件

时间日期处理插件:

* `moment.js`    antd版本<=4
* `dayjs`    antd版本>=5
  * 体积小「2KB，moment貌似16kb」
  * 用的API方法，和moment类似
  * 更符合国际化日期处理规范
  * .….

### 利用Hooks组件改造

![image-20240115215513652](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401152155955.png)

触发Form表单校验的方式:

1. 前提:提交按钮包裹在`<Form>`中，并且htmlType='submit'点击这个按钮,会自动触发Form的表单校验
   表单校验通过，会执行`<Form onFinish={函数}>`事件

   * 函数执行，形参获取的就是表单收集的信息

2. 我们获取Form组建的实例[或者是子组件内部返回的方法]

   基于这些方法，触发表单校验&获取表单手机的信息等

   * `validateFields`
   * `getFieldsValue`
   * `resetFields`

ant-designer提供的独有的获取表单ref方法 **只适用于函数式组件**

```jsx
<Form form={formRef}></Form>
// 使用   
let [formRef] = Form.useForm(); // 表单ref
```

![image-20240115221455188](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401152214457.png)

> 函数组件中，遇到:修改某个状态后(视图更新后)，想去做一些事情(而这些事情中，**需要用到新修改的状态值**)
> 此时:我们不能直接在代码的下面编写，或者把修改状态改为同步的，这些都不可以!因为只有在函数重新执行，产生的新的闭包中，才可以获取最新的状态值! !**原始闭包中用的还是之前的状态值**! !
>
> 解决方案：**基于useEffect设置状态的依赖**， 在依赖的状态发生改变后，去做要做的事情! !
>
> 修改某个状态后(视图更新后)，想去做一些事情，但是要处理的事情，和新的状态值没有关系，此时可以把修改状态的操作，基于`flushSync`变为同步处理即可! 

完整代码：


```jsx
import React, { useEffect, useState, useRef } from 'react'
import { Button, Tag, Table, Popconfirm, Modal, Form, Input, DatePicker, message } from 'antd';
import '../assets/css/task.scss'
import { getTaskList, addTask, removeTask, completeTask } from '../api';


const zero = function zero(text) {
  text = String(text);
  return text.length < 2 ? '0' + text : text;
}
const formatTime = function formatTime(time) {
  let arr = time.match(/\d+/g);
  let [, month, day = '00', hours, minutes = '00'] = arr;
  return `${zero(month)}-${zero(day)} ${zero(hours)}:${zero(minutes)}`;
}

const Task = function Task() {
  /* 表格列的数据 */
  const columns = [
    {
      title: '编号',
      dataIndex: 'id',
      align: 'center',
      width: '8%'
    },
    {
      title: '任务描述',
      dataIndex: 'task',
      ellipsis: true,
      width: '50%'
    },
    {
      title: '状态',
      dataIndex: 'state',
      align: 'center',
      width: '10%',
      render: text => +text === 1 ? <span style={{color: 'red'}}>未完成</span> : <span style={{color: 'green'}}>已完成</span>
    },
    {
      title: '完成时间',
      dataIndex: 'time',
      align: 'center',
      width: '15%',
      render: (_, record) => {
        let { state, time, complete } = record;
        if (+state === 2) time = complete;
        return formatTime(time)
      }
    }, 
    {
      title: '操作',
      render: (_, record) => {
        let { id, state } = record;
        return <>
          <Popconfirm
            title="您确定要删除此任务吗"
            onConfirm={deleteTask.bind(this, id)}
          >
            <Button type="link">删除</Button>
          </Popconfirm>
          
          {
            +state !== 2 ? <Popconfirm title="您确定要完成此任务吗" onConfirm={updateTaskState.bind(null, id)}>
              <Button type="link">完成</Button>
            </Popconfirm> : null
          }
        </>
      }
    }
  ]

   // 分页配置信息
   const pageProps = {
    current: 1, // 当前页数
    pageSize: 2, // 每页条数
    showSizeChanger: true, // 显示分页切换器
    showQuickJumper: true, // 显示快速跳转至某页
    pageSizeOptions: [1,2,5,10],
    total: 0,
    showTotal: (total, range) => `${range[0]}-${range[1]} 共 ${total} 条`,
    onChange: (page, pageSize) => pageChange(page, pageSize)
  }

  let [tableData, setTableData] = useState([]); // 表格数据
  let [pageInfo, setPageInfo] = useState(pageProps); // 分页信息
  let [modalVisible, setModalVisible] = useState(false); // 弹窗是否显示
  let [tableLoading, setTableLoading] = useState(false); // 表格loading
  let [saveTaskconfirmLoading, setSaveTaskconfirmLoading] = useState(false); // 提交任务loading
  let [selectIndex, setSelectIndex] = useState(0); // 选中项的索引


  // let formRef = useRef(null); // 表单ref
  // 这种方式ref = {formRef} 获取实例： formRef.current
 
  // ant-design提供的
   let [formRef] = Form.useForm(); // 表单ref


  // 页码切换事件
  const pageChange = (page, pageSize) => {
    setPageInfo({
      ...pageInfo,
      current: page,
      pageSize
    })
    // queryData()
    // console.log('页码发生变化')
  }

  useEffect(() => {
    queryData()
    // console.log('分页信息发生改变')
  }, [pageInfo.current, pageInfo.pageSize, selectIndex])


  // 完整状态切换
  const changeIndex = (index) => {
    // 这个可以不要，因为useState存在性能优化，值相同视图不更新
    // if(selectIndex === index) return
    
    // 这样写是错误的，因为闭包问题，queryData获取的上下文的setSelectIndex还是上次的
    // flushSync(() => {
    //   setSelectIndex(index)
    // })
    // queryData();
    setSelectIndex(index);
    setPageInfo((prev) => {
      return {
        ...prev,
        current: 1
      }
    })
  }

  // 获取任务列表
  const queryData = async () => {
    setTableLoading(true)
    try {
      let { code, list, page, total } = await getTaskList(selectIndex, pageInfo.current, pageInfo.pageSize);
      if(+code !== 0) { // 0代表获取成功
        list = []
      }
      console.log('请求获取到的数据', list)
      setTableData(list)
      setPageInfo({
        ...pageInfo,
        total: +total
      })
    } catch (error) {
      message.error('获取任务列表失败')
    }
    setTableLoading(false)
  }

  // 删除任务
  const deleteTask = async (id) => {
    let { code } = await removeTask(id);
    if(+code !== 0) {
      message.error('删除任务失败')
      return
    } else {
      queryData()
      message.success('删除任务成功')
    } 
  }

  // 修改任务状态
  const updateTaskState = async (id) => {
    let {code} = await completeTask(id);
    if(+code !== 0) {
      message.error('修改任务状态失败')
      return
    } else {
      queryData()
      message.success('修改任务状态成功')
    } 
  }

  // 提交任务
  const saveTask = async () => {
    try {
      // 表单校验
      await formRef.validateFields()
      let {task , time} = formRef.getFieldsValue();
      time = time.format('YYYY-MM-DD HH:mm:ss');
      setSaveTaskconfirmLoading(true)
      // 向服务器端发送请求
      let { code } = await addTask(task, time);
      if(+code !== 0) {
        message.error('添加任务失败');
      } else {
        // 关闭弹框
        closeMode();
        // 获取最新的数据
        queryData();
        message.success('添加任务成功');
      }
    }catch (_) {
      message.error('请填写完整信息');
    }
    setSaveTaskconfirmLoading(false)
  }

  // 关闭弹框事件
  const closeMode = () => {
    setModalVisible(false)
    setSaveTaskconfirmLoading(false)
    formRef.resetFields();
  }

  // 页面第一次加载，发送请求获取数据
  useEffect(() => {
    queryData();
    console.log('获取到的表格数据', tableData)
  }, [])



  return <div className='task_box'>
    <div className="task_header">
      <h1>TASK OA任务管理系统</h1>
      <Button type="primary" onClick={() => {
        setModalVisible(true)
      }}>新增任务</Button>
      </div>
      <div className='task_content'>
        <div className='task_content_header'>
          {['全部', '未完成', '已完成'].map( (item, index) => {
            return <Tag color={selectIndex === index ? '#1677ff' : ''} key={index} onClick={changeIndex.bind(null, index)}>{item}</Tag>
          })}
        </div>
        <div className='task_content_table'>
          <Table dataSource={tableData} loading={tableLoading} columns={columns}  rowKey="id" pagination={pageInfo}/>
        </div>
      </div>
      {/* 新增任务弹出框 */}
      <Modal title="新增任务窗口" open={modalVisible} maskClosable={false} okText="提交信息" onCancel={closeMode} onOk={saveTask} confirmLoading={saveTaskconfirmLoading}>
        <Form form={formRef} layout="vertical" initialValues={{ task: '', time: '' }} validateTrigger="onBlur">
          <Form.Item label="任务描述" name="task" rules={[
            { required: true, message: '任务描述是必填项' }, 
            { min: 6, message: '输入的内容至少6位及以上' }
          ]}>
            <Input.TextArea rows={4} />
          </Form.Item>
          <Form.Item label="任务预期完成时间" name="time" rules={[
            { required: true, message: '预期完成时间是必填项' }
          ]}>
            <DatePicker showTime />
          </Form.Item>
        </Form>
      </Modal>
  </div>
}

export default Task;
```



## Hooks 组件

### React组件分类

- 函数组件
  - **不具备“状态、ref、周期函数”**等内容，第一次渲染完毕后，无法基于组件内部的操作来控制其更新，因此称之为静态组件！
  - 但是具备属性及插槽，父组件可以控制其重新渲染！
  - 渲染流程简单，渲染速度较快！
  - 基于FP（函数式编程）思想设计，提供更细粒度的逻辑组织和复用！
- 类组件
  - **具备“状态、ref、周期函数、属性、插槽”**等内容，可以灵活的控制组件更新，基于钩子函数也可灵活掌控不同阶段处理不同的事情！
  - 渲染流程繁琐，渲染速度相对较慢！
  - 基于OOP（面向对象编程）思想设计，更方便实现继承等！

React Hooks 组件，就是基于 React 中新提供的 Hook 函数，可以`让函数组件动态化`!

### Hook函数概览

官方文档：https://zh-hans.react.dev/learn#using-hooks

- 基础 Hook
  - `useState` 使用状态管理
  - `useEffect` 使用周期函数
  - `useContext` 使用上下文信息
- 额外的 Hook
  - `useReducer` useState的替代方案，借鉴redux处理思想，管理更复杂的状态和逻辑
  - `useCallback` 构建缓存优化方案
  - `useMemo` 构建缓存优化方案
  - `useRef` 使用ref获取DOM
  - `useImperativeHandle` 配合forwardRef（ref转发）一起使用
  - `useLayoutEffect` 与useEffect相同，但会在所有的DOM变更之后同步调用effect
  - …
- 自定义Hook
- ……

### useState

> 作用：在函数组件中使用状态，修改状态值可让函数组件更新，类似于类组件中的setState
> 语法：
> `const [state, setState] = useState(initialState);`
> 返回一个 state，以及更新 state 的函数

#### 基本使用

> `useState`：React Hook函数之一，目的是在函数组件中使用状态，并且后期基于状态的修改，可以让组件更新
> `let [num,setNum] = useState(initialValue);`
>
>    + 执行useState，传递的initialValue是初始的状态值
>      +  执行这个方法，返回结果是一个数组：[状态值,修改状态的方法]
>         
>        *  num变量存储的是：获取的状态值
>        
>        + setNum变量存储的是：修改状态的方法
> + 执行 setNum(value) 
>      + 修改状态值为value
>      + 通知视图更新
>
> 函数组件「或者Hooks组件」不是类组件，所以没有实例的概念「调用组件不再是创建类的实例，而是把函数执行，产生一个私有上下文而已」，再所以，在函数组件中不涉及this的处理！！

```jsx
import React, { useState } from "react";

export default function Demo() {
  let [num, setNum] = useState(0);
  const handler = () => {
    setNum(num + 1);
  }
  return <>
    <h1>num: {num}</h1>
    <button onClick={handler}>增加</button>
  </>
}
```

#### 设计原理

函数组件的更新是让函数重新执行，也就是useState会被重新执行；那么它是如何确保每一次获取的是最新状态值，而不是传递的初始值呢？

```jsx
export default function Demo() { 
  let [num, setNum] = useState(10);
  const handler = () => {
      setNum(100);
      setTimeout(() => {
          console.log(num); //10
      }, 1000);
  };
  return <div>
      <span>{num}</span>
      <button onClick={handler}>新增</button>
  </div>;
}
```

![image-20240114155944631](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401141559758.png)

#### 实现原理

```js
var _state;
function useState(initialValue) {
  // 这样保证了初始值只会被赋值一次
  if(typeof _state === 'undefined') {
    if(typeof initialValue === "function") {
      _state = initialValue();
    } else {
      _state = initialValue;
    }
  }
  var setState = function setState(value) {
    // 两个值相同，不更新视图
    if(Object.is(value, _state)) return;
    if(typeof value === 'function') {
      _state = value(_state);
    } else {
      _state = value;
    }
    // 通知视图更新
  }
  return [_state, setState];
  // 返回一个数组，第一个是当前状态，第二个是更新状态的函数
}
let [num1, setNum] = useState(0); //num1=0  setNum=setState 0x001
setNum(100); //=>_state=100 通知视图更新
// ---
let [num2, setNum] = useState(0); //num2=100  setNum=setState 0x002
```

![image-20240114155344027](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401141553216.png)

#### 更新多状态

方案一：类似于类组件中一样，让状态值是一个对象（包含需要的全部状态），每一次只修改其中的一个状态值！
问题：不能像类组件的setState函数一样，支持部分状态更新！

```jsx
import React, {useState} from 'react'

export default function Demo() {
  let [state, setState] = useState({
      x: 10,
      y: 20
  });
  const handler = () => {
      // setState({ x: 100 }); //state={x:100} // 这样y会丢失
      setState({
          ...state, 
          x: 100
      });
  };
  return <div>
      <span>x:{state.x}</span><br/>
      <span>y:{state.y}</span>
      <button onClick={handler}>处理</button>
  </div>;
}
```

方案二：执行多次useState，把不同状态分开进行管理「推荐方案」

```jsx
export default function Demo() {
  let [x, setX] = useState(10);
  let [y, setY] = useState(20); 
  const handler = () => {
    setX(100);
  };
  return <div>
      <span>x:{x}</span><br/>
      <span>y:{y}</span>
      <button onClick={handler}>处理</button>
  </div>;
}
```

#### 更新队列机制

和类组件中的setState一样，每次更新状态值，也不是立即更新，而是加入到更新队列中！

- React 18 全部采用批更新
- React 16 部分批更新，放在其它的异步操作中，依然是同步操作！
- 可以基于flushSync刷新渲染队列

![image-20240113215607763](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401132156047.png)

![image-20240113215824212](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401132158328.png)

```jsx
import React, { useState } from 'react'
import { flushSync } from 'react-dom'

export default function Demo() {
  console.log('RENDER渲染');
  let [x, setX] = useState(10),
      [y, setY] = useState(20),
      [z, setZ] = useState(30);

  const handle = () => {
      flushSync(() => {
          setX(x + 1);
          setY(y + 1);
      });
      setZ(z + 1);
  };
  return <div className="demo">
      <span className="num">x:{x}</span><br/>
      <span className="num">y:{y}</span><br/>
      <span className="num">z:{z}</span><br/>
      <button type="primary"
          size="small"
          onClick={handle}>
          新增
      </button>
  </div>;
}
```

![image-20240113220008234](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401132200372.png)

> 在React16中，也和this.setState一样，放在**合成事件/周期函数中，其实异步**的操作;但是放在其它的异步操作中「例如:定时器、手动的事件绑定等」它是**同步**的

![image-20240113220337510](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401132203652.png)

#### 函数式更新

![image-20240113221115132](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401141646433.png)

> 上述代码 最终只会渲染一次render， 最终x是11

如果新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 setState；该函数将接收先前的 state，并返回一个更新后的值！

```jsx
export default function Demo() {
  console.log('RENDER渲染');
  let [x, setX] = useState(10);

  const handle = () => {
    for(let i = 0; i < 10; i++) {
      setX(prev => {
        // prev:存储上一次的状态值
        console.log(prev);
        return prev + 1; //返回的信息是我们要修改的状态值
      })
    }
  };
  return <div className="demo">
      <span>x: {x}</span>
      <button type="primary"
          size="small"
          onClick={handle}>
          执行
      </button>
  </div>;
}
```

#### 性能优化

> useState自带了性能优化的机制:
>
> * 每一次修改状态值的时候，会拿最新要修改的值和之前的状态值做比较「基于Object.is作比较」
> * 如果发现两次的值是一样的，则不会修改状态，也不会让视图更新「可以理解为︰类似于PureComponent，在shouldComponentUpdate中做了浅比较和优化」

![image-20240113223627999](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401132236214.png)

#### 惰性初始state

如果初始 state 需要通过复杂计算获得，则可以传入一个函数，在函数中计算并返回初始的 state，此函数只在初始渲染时被调用！

```jsx
	let [num, setNum] = useState(() => {
        let { x, y } = props;
        return x + y;
    });
```

> 此时我们需要对初始值的操作，进行惰性化处理：只有第一次渲染组件处理这些逻辑，以后组件更新，这样的逻辑就不要再运行了！！

### useEffect

> 作用：在函数组件中使用生命周期函数
> 语法：具备很多情况
> `useEffect([callback],[dependencies])`

 useEffect：在函数组件中，使用生命周期函数

* `useEffect(callback)`：没设置依赖
     + **第一次渲染完毕**后，执行callback，等价于 **componentDidMount**
     + 在组件**每一次更新完毕后**，也会执行callback，等价于 **componentDidUpdate**
   * `useEffect(callback,[])`：设置了，但是无依赖
             + 只有**第一次渲染完毕后，才会执行**callback，每一次视图更新完毕后，callback不再执行
                  + 类似于 **componentDidMount**
   * `useEffect(callback,[依赖的状态(多个状态)])`：
           + **第一次渲染完毕会执行**callback
           + 当依赖的**状态值(或者多个依赖状态中的一个)发生改变**，也会触发callback执行
             + 但是依赖的状态如果没有变化，在组件更新的时候，callback是不会执行的

* `useEffect(()=>{return 函数})  `

  ```jsx
  useEffect(()=>{
        return ()=>{
          // 返回的小函数，会在组件释放的时候执行
          // 如果组件更新，会把上一次返回的小函数执行「可以“理解为”上一次渲染的组件释放了」
        };
     });
  ```

```jsx
import React, { useEffect, useState } from 'react'

export default function Demo() {
  let [x, setX] = useState(0),
  [num, setNum] = useState(10);

  useEffect(() => {
    // 第一次渲染完成和实体更新都会触发
    // 类似 componentDidMount && componentDidUpdate
    console.log('@1', num)
  })

  useEffect(() => {
    // 第一次渲染完成触发
    // 类似 componentDidMount
    console.log('@2', num)
  }, [])

  useEffect(() => {
    // 所依赖的num状态发生改变触发
    console.log('@3', num)
  }, [num])

  useEffect(() => {
    return () => {
    // 组件释放执行
      // 获取的值还是上一次的
      console.log('@4', num);
    }
  })

  const handle = () => {
    setNum(num + 1);
  };
  const handle2 = () => {
    setX(x + 1);
  }
  return <>
      <span>{num}</span>
      <button onClick={handle}>新增num</button>
      <button onClick={handle2}>新增x</button>
  </>;
}
```

#### useEffect的原理

函数组件在渲染（或更新）期间，遇到useEffect操作，会基于MountEffect方法把callback（和依赖项）加入到`effect链表`中！

在视图渲染完毕后，基于UpdateEffect方法，通知链表中的方法执行！
1、按照顺序执行期间，首先会检测依赖项的值是否有更新「有容器专门记录上一次依赖项的值」；有更新则把对应的callback执行，没有则继续处理下一项！！
2、遇到依赖项是空数组的，则只在第一次渲染完毕时，执行相应的callback
3、遇到没有设置依赖项的，则每一次渲染完毕时都执行相应的callback

……

![image-20240114210717120](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401142107621.png)

#### 注意事项

只能在函数最外层调用 Hook，不要在循环、条件判断或者子函数中调用。

```jsx
import React, {useEffect, useState} from 'react'

// 模拟从服务器端获取数据
const queryData = () => {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve([10, 20, 30])
    }, 2000)
  })
}

export default function Demo() {
  let [num, setNum] = useState(0);

  // 这样会报错
  // if(num >= 5) {
  //   useEffect(() => {
  //     console.log('执行');
  //   })
  // }

  useEffect(() => {
    if(num >= 5) {
      console.log('执行');
    }
  })

  // 这样会报错， 加了async后，返回的是一个promise函数
  // useEffect如果设置返回值，则返回值必须是一个函数「代表组件销毁时触发」;
  // 下面案例中，callback经过async的修饰，返回的是一个promise实例，不符合要求！！
  // useEffect(async () => {
  //   let res = await queryData();
  //   console.log(res)
  // }, [])

  // 推荐用下面的方法
  useEffect(() => {
    queryData().then(res => {
      console.log(res)
    })
  }, [])

  // 或者这种方式也行
  useEffect(() => {
    const next = async () => {
      let res = await queryData()
      console.log(res)
    }
    next()
  })

  const handle = () => {
    setNum(num + 1); // 更新num的值
  }
  return <>
    <span>{num}</span>
    <button onClick={handle}>新增num</button>
  </>;
}
```

#### 异步获取数据

不能直接对[callback]设置async，因为它只能返回一个函数（或者不设置返回值）

```jsx
import React, { useState, useEffect } from "react";
const queryData = () => {
    return fetch('/api/subscriptions/recommended_collections')
        .then(response => {
            return response.json();
        });
};
export default function Demo() {
    let [data, setData] = useState([]);
    /* // Warning: useEffect must not return anything besides a function, which is used for clean-up.
    useEffect(async () => {
        let result = await queryData();
        setData(result);
        console.log(result);
    }, []); */
    useEffect(() => {
        const next = async () => {
            let result = await queryData();
            setData(result);
            console.log(result);
        };
        next();
    }, []);
    return <div>
        ...
    </div>;
};
```

### useLayoutEffect

* 其函数签名与 useEffect 相同，但它会在所有的 DOM 变更之后同步调用 effect。
* 可以使用它来读取 DOM 布局并同步触发重渲染。
* 在浏览器执行绘制之前，useLayoutEffect 内部的更新计划将被同步刷新。
* 尽可能使用标准的 useEffect 以避免阻塞视图更新。

```jsx
import React, { useState, useEffect, useLayoutEffect } from "react";


const Demo = function Demo() {
    // console.log('RENDER');
    let [num, setNum] = useState(0);

    // useLayoutEffect(() => {
    //     if (num === 0) {
    //         setNum(10);
    //     }
    // }, [num]); 

    useLayoutEffect(() => {
        console.log('useLayoutEffect'); //第一个输出
    }, [num]);
    useEffect(() => {
        console.log('useEffect'); //第二个输出
    }, [num]);

    return <div
        style={{
            backgroundColor: num === 0 ? 'red' : 'green'
        }}>
        <span>{num}</span>
        <button onClick={() => {
                setNum(0);
            }}>
            新增
        </button>
    </div>;
};

export default Demo;
```

![image-20240114193311496](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401141933776.png)

![image-20240114195007002](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401141950157.png)

> `useLayoutEffect`**会阻塞浏览器渲染真实DOM**，优先执行Effect链表中的callback；
>
> `useEffect`**不会阻塞浏览器渲染真实DOM**，在渲染真实DOM的同时，去执行Effect链表中的callback；
>
>    + `useLayoutEffect`设置的callback要**优先**于`useEffect`去执行！！
>    + 两者设置的callback中，依然可以获取DOM元素「原因：真实DOM对象已经创建了，区别只是浏览器是否渲染」
>    + 如果在callback函数中又修改了状态值「视图又要更新」
>         + useEffect:浏览器肯定是把第一次的真实已经绘制了，再去渲染第二次真实DOM
>         + useLayoutEffect:浏览器是把两次真实DOM的渲染，合并在一起渲染的
>
>  **视图更新的步骤**：
>
> * 第一步：基于babel-preset-react-app把JSX编译为createElement格式
>
> *  第二步：把createElement执行，创建出virtualDOM
>
> * 第三步：基于root.render方法把virtualDOM变为真实DOM对象「DOM-DIFF」
>   * useLayoutEffect阻塞第四步操作，先去执行Effect链表中的方法「同步操作」
>   * useEffect第四步操作和Effect链表中的方法执行，是同时进行的「异步操作」
> *  第四步：浏览器渲染和绘制真实DOM对象

### useRef

类组件中，我们基于ref可以做的事情:

* 赋值给一个标签︰获取DOM元素
* 赋值给一个类子组件:获取子组件实例「可以基于实例调用子组件中的属性和方法等」
* 赋值给一个函数子组件:报错「需要配合React.forwardRet实现ref转发，获取子组件中的摸一个DOM元素」

#### 类组件中ref的使用

ref的使用方法:

* `ref='box'`

  this.refs.box 获取{不推荐使用}

* `ref={x=>this.box=x}`

  this.box获取

* `this.box=React.createRef()`创建一个ref对象

  ==<h2 ref={this.box}>==
  this.box.current获取DOM元素

#### Hooks函数组件

在函数组件中，可以基于`useRef`获取DOM元素！类似于类组件中的 ：

- `ref={x=>thix.box=x}`
- `React.createRef`

> `useRef` 只能在函数组件中用「所有的ReactHook函数，都只能在函数组件中时候用，在**类组件中使用会报错**」

```jsx
function Demo() {
  let x = useRef(null);
  let y = React.createRef();
  let z;
  useEffect(() => {
    console.log(x.current, y.current, z)
  }, [])
  return <>
  {/* // Function components cannot have string refs. We recommend using useRef() instead.
  <span ref="box"></span> */}
    <ChildDemo1 ref={x}></ChildDemo1>
    <p ref={y}>父组件的内容</p>
    <div ref={x => z = x}>这样也可以获取ref</div>
  </>
}

// 基于forwardRef实现ref转发，目的：获取子组件内部的某个元素
const ChildDemo1 = React.forwardRef(function ChildDemo1(props, ref) {
  // console.log(ref)
  return <>
    <span ref={ref}>子组件1</span>
  </>
})

export default Demo
```

> 注意：`React.createRef`在函数组件中依然可以使用！
>
> - `createRef` 每次渲染都会返回**一个新的引用**
> - 而` useRef `每次都会返回**相同的引用**

```jsx
let prev1;
let prev2;
const ChildDemo2 = function ChildDemo2() {
  let [num, setNum] = useState(0);
  let x1 = useRef(null);
  let x2 = React.createRef();

  if(!prev1) {
    // 第一次DEMO执行，把第一次创建的REF对象赋值给变量
    prev1 = x1;
    prev2 = x2;
  } else {
    console.log('prev1 === x1', prev1 === x1); // useRef再每一次组件更新的时候（函数重新执行），再次执行useRef方法的时候，不会创建新的REF对象了，获取到的还是第一次创建的那个REF对象！！
    console.log('prev2 === x2', prev2 === x2); // false createRef在每一次组件更新的时候，都会创建一个全新的REF对象出来，比较浪费性能！！
    // 总结：在类组件中，创建REF对象，我们基于 React.createRef 处理；但是在函数组件中，为了保证性能，我们应该使用专属的 useRef 处理！！
  }

  return (<>
    <div>测试useRef和React.createRef区别</div>
    <span>num: {num}</span>
    <button onClick={
      () => {
        setNum(num + 1)
      }
    }>新增</button>
  </>)
}
```

### useImperativeHandle

> useImperativeHandle 可以让你在使用 **ref 时自定义暴露给父组件的实例值**，应当与 forwardRef 一起使用，实现ref转发！

```jsx
import React, { useImperativeHandle, useRef, useState } from "react";

class ClassChild extends React.Component {
   state = {
    num: 1
   }

  submit = () => {
    console.log('调用了子类的方法')
  }
  add = () => {
    this.setState({
      num: this.state.num + 1
    })
  }
  render() {
    let { num } = this.state
    return <>
      <h2>类组件</h2>
      <p>num: {num} </p>
      <button onClick={this.add}>子组件自调用点击加加</button>
    </>
  }
}

const FunDemo = React.forwardRef(function FunDemo(props, ref) {
  let [num, setNum] = useState(0)
  useImperativeHandle(ref, () => {
    // 在这里返回的内容，都可以被父组件的REF对象获取到
    return {
      num,
      add
    }
  })

  const add = () => {
    console.log('函数子组件add方法执行')
    setNum(num + 1)
  }
  return <>
    <h2>函数子组件</h2>
    <p>num: {num} </p>
    <button onClick={add}>子组件自调用点击加加</button>
  </>
})

const Demo = function Demo() {
  let child1 = useRef(null);
  let child2 = useRef(null);
  return <>
    <h1>函数式父组件</h1>
    <button onClick={() => {
      child1.current.add()
    }}>调用类组件中的方法</button>
    <button onClick={() => {
      child2.current.add()
    }}>调用函数组件中的方法</button>
    <div style={{width: 300, height: 300, border: '1px solid #ccc'}}>
      <ClassChild ref={child1}></ClassChild>
    </div>
    <div style={{width: 300, height: 300, border: '1px solid #ccc'}}>
      <FunDemo ref={child2}></FunDemo>
    </div>
    
  </>
}


export default Demo;
```

![image-20240115205712806](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401152057320.png)

### useMemo

在前端开发的过程中，我们需要缓存一些内容，以避免在需渲染过程中因大量不必要的耗时计算而导致的性能问题。为此 React 提供了一些方法可以帮助我们去实现数据的缓存，useMemo 就是其中之一！

> `let xxx = useMemo(callback,[dependencies])`
>
>    + 第一次渲染组件的时候，callback会执行
>    + 后期只有依赖的状态值发生改变，callback才会再执行
>    + 每一次会把callback执行的返回结果赋值给xxx
>    + useMemo具备“计算缓存”，在依赖的状态值没有发生改变，callback没有触发执行的时候，xxx获取的是上一次计算出来的结果
>       和Vue中的计算属性非常的类似！！

![image-20240117161048101](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401171610343.png)

```jsx
import React, { useEffect, useMemo, useState } from "react";

export default function Demo() {
  let [supNum, setSupNum] = useState(0);
  let [oppNum, setOppNum] = useState(0);
  let [x, setX] = useState(0);
  

  // // 计算支持比率
  // let total = supNum + oppNum, ratio = '--';
  // if(total > 0) {
  //   console.log('当x++的时候，我也会执行')
  //   ratio = (supNum / (total)).toFixed(2);
  // }

  let ratio = useMemo(() => {
    let ratio = '--';
    let total = supNum + oppNum;
    if(total > 0) {
      ratio = (supNum / (total)).toFixed(2);
    }
    console.log('我只有在supNum和oppNum改变的时候，才会触发')
    return ratio;
  }, [supNum, oppNum])

  // let ratio = '--';
  // let [ratio, setRatio] = useState('--');
  // useEffect(() => {
  //   let total = supNum + oppNum;
  //   if(total > 0) {
  //     ratio = (supNum / (total)).toFixed(2);
  //   }
  //   setRatio(ratio) // 需要添加这个才会重新渲染
  //   console.log('支持率将重新计算，但是ratio是不会重新渲染的')
  // }, [supNum, oppNum])

  
  
  return <>
    <div className="main">
      <p>支持人数：{supNum}人</p>
      <p>反对人数：{oppNum}人</p>
      <p>支持比率：{ratio}</p>
      <p>x: {x}</p>
    </div>
    <div className="footer">
      <button onClick={() => {
        setSupNum(supNum + 1);
      }}>支持</button>
      <button onClick={() => {
        setOppNum(oppNum + 1);
      }}>反对</button>
      <button onClick={() => {
        setX(x + 1);
      }}>x++</button>
    </div>
  </>
}
```

![image-20240117163115193](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401171631397.png)


> 比较两种方法的主要区别是，`useMemo`适用于记忆函数的计算结果，而`useEffect`用于处理副作用。在某些情况下，`useMemo`更适合计算值的情景，因为它是专门为此而设计的。而`useEffect`则更适用于**处理那些不直接影响渲染结果**但需要在数据变化时执行的任务。

### useCallback

useCallback 用于得到一个固定引用值的函数，通常用它进行性能优化！

>  `const xxx = useCallback(callback,[dependencies])`
>    + 组件第一次渲染，useCallback执行，创建一个函数“callback”，赋值给xxx
>    + 组件后续每一次更新，判断依赖的状态值是否改变，如果改变，则重新创建新的函数堆，赋值给xxx；但是如果，依赖的状态没有更新「或者没有设置依赖“[]”」则xxx获取的一直是第一次创建的函数堆，不会创建新的函数出来！！
>    + 或者说，基于useCallback，可以始终获取第一次创建函数的堆内存地址(或者说函数的引用)

![image-20240117164256263](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401171642405.png)
诉求：当父组件更新的时候，因为传递给子组件的属性仅仅是一个函数「特点：基本应该算是不变的」，所以不想再让子组件也跟着更新了！

* 第一条：传递给子组件的属性（函数），每一次需要是相同的堆内存地址(是一致的)。基于useCallback处理！！
* 第二条：在子组件内部也要做一个处理，验证父组件传递的属性是否发生改变，如果没有变化，则让子组件不能更新，有变化才需要更新。继承`React.PureComponent`即可「在shouldComponentUpdate中对新老属性做了浅比较」!! **函数组件**是基于 `React.memo` 函数，对新老传递的属性做比较，如果不一致，才会把函数组件执行，如果一致，则不让子组件更新！！

```jsx
  // const handler = () => {} //第一次:0x001  第二次:0x101  第三次:0x201 ...
  const handler = useCallback(() => {}, []) //第一次:0x001  第二次:0x001  第三次:0x
  if(!prev) {
    prev = handler;
  } else {
    console.log(prev === handler) // 每次都会创建新的函数
  }
```

![image-20240118163417386](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401181634861.png)

```jsx
class Child extends React.PureComponent {
  render() {
    console.log('类子组件渲染');
    return <div>类子组件</div>;
  }
}

const Child2 = React.memo(function Child2() {
  console.log('函数子组件渲染')
  return <>
    <div>函数子组件</div>
  </>
})


let prev;
export default function Demo() {
  let [x, setX] = useState(0);

  // const handler = () => {} //第一次:0x001  第二次:0x101  第三次:0x201 ...
  const handler = useCallback(() => {}, []) //第一次:0x001  第二次:0x001  第三次:0x
  if(!prev) {
    prev = handler;
  } else {
    console.log(prev === handler) // 每次都会创建新的函数
  }
  const add = () => {
    setX(x++);
  }
  return <>
    <p>x: {x}</p>
    <Child handler={handler}></Child>
    <Child2 handler={handler}></Child2>
    <button onClick={add}>x++</button>
  </>
}
```

![image-20240118163548033](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401181635158.png)

### 自定义Hook

使用自定义hook可以将某些组件逻辑提取到可重用的函数中

```jsx
import React, { useEffect, useState } from 'react'

/* 
自定义Hook 
  作用：提取封装一些公共的处理逻辑
  玩法：创建一个函数，名字需要是 useXxx ，后期就可以在组件中调用这个方法！
*/
const usePartialState = function usePartialState(initialValue) {
  let [state, setState] = useState(initialValue);
  // setState:不支持部分状态更改的
  // setPartial:我们期望这个方法可以支持部分状态的更改
  const setPartial = function setPartial(partialState) {
    setState((state) => ({ ...state, ...partialState }));
  }
  return [state, setPartial];
}

// 自定义Hook，在组件第一次渲染完毕后，统一干点啥事
const useDidMount = function useDidMount(title) {
  if(!title) title = '哈哈哈'
  // 基于React内置的Hook函数，实现需求即可
  useEffect(() => {
    document.title = title;
  }, [])
}



export default function Demo() {
  // let [state, setState] = useState({
  //   supNum: 10,
  //   oppNum: 5
  // })
  // 如果是对象形式，修改其中的一个值，必须把原来的值赋值一份，否则会其他值会丢失
  // const handle = (type) => {
  //   if(type === 'sup') {
  //     setState({
  //       ...state,
  //       supNum: state.supNum + 1
  //     })  
  //   } else {
  //     setState({
  //       ...state,
  //       oppNum: state.oppNum + 1
  //     })
  //   }
  // }
  let [state, setPartial] = usePartialState({
      supNum: 10,
      oppNum: 5
  });

  const handle = (type) => {
      if (type === 'sup') {
          setPartial({
              supNum: state.supNum + 1
          });
          return;
      }
      setPartial({
          oppNum: state.oppNum + 1
      });
  };

  useDidMount('哈哈哈哈哈');
  return <div className="vote-box">
  <div className="main">
      <p>支持人数：{state.supNum}人</p>
      <p>反对人数：{state.oppNum}人</p>
  </div>
  <div className="footer">
      <button type="primary" onClick={handle.bind(null, 'sup')}>支持</button>
      <button type="primary" danger onClick={handle.bind(null, 'opp')}>反对</button>
  </div>
</div>;
}
```

## React 复合组件通信方案

### 基于props属性，实现父子(或兄弟)组件间的通信

![下载](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401202056305.png)

基本结构

![image-20240119194428368](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401191944516.png)

#### 类组件

![image-20240119195321895](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401191953075.png)

Vote.jsx

```jsx
import React from "react";
import './Vote.less';
import VoteMain from './VoteMain';
import VoteFooter from './VoteFooter';

class Vote extends React.Component {
    state = {
        supNum: 10,
        oppNum: 0
    }
    // 设置为箭头函数：不论方法在哪执行的，方法中的this永远都是Vote父组件的实例
    change = (type) => {
        let { supNum, oppNum} = this.state
        if(type === 'sup') {
            this.setState({
                supNum: supNum + 1
            })
            return;
        }
        this.setState({oppNum: oppNum + 1})
    }
    render() {
        let {supNum, oppNum} = this.state;
        return <div className="vote-box">
            <div className="header">
                <h2 className="title">React是很棒的前端框架</h2>
                <span className="num">{supNum + oppNum}</span>
            </div>
            <VoteMain supNum={supNum} oppNum={oppNum} />
            <VoteFooter change={this.change} />
        </div>;
    }
}

export default Vote;
```

VoteMain.jsx

```jsx
import React from "react";
import PropTypes  from 'prop-types'


class VoteMain extends React.Component {
    /* 属性规则校验 */
    static defaultProps = {
        supNum: 0,
        oppNum: 0
    };
    static propTypes = {
        supNum: PropTypes.number.isRequired,
        oppNum: PropTypes.number.isRequired
    };

    render() {
        let { supNum, oppNum } = this.props;
        console.log(supNum, oppNum);
        let ratio = '--',
        total = supNum + oppNum;
        if (total > 0) ratio = (supNum / total * 100).toFixed(2) + '%';
        return <div className="main">
            <p>支持人数：{supNum}人</p>
            <p>反对人数：{oppNum}人</p>
            <p>支持比率：{ratio}</p>
        </div>;
    }
}

export default VoteMain;
```

VoteFooter.jsx

```jsx
import React from "react";
import { Button } from 'antd';

class VoteFooter extends React.Component {
    render() {
        let {change} = this.props;
        return <div className="footer">
            <Button type="primary" onClick={change.bind(null, 'sup')}>支持</Button>
            <Button type="primary" onClick={change.bind(null, 'opp')} danger>反对</Button>
        </div>;
    }
}

export default VoteFooter;
```

![image-20240119195920023](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401191959307.png)

![image-20240119200350935](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401192003162.png)

![image-20240119200728342](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401192007493.png)

> 理解一：属性的传递方向是单向的
>
> - 父组件可基于属性把信息传给子组件
> - 子组件无法基于属性给父组件传信息；但可以把父组件传递的方法执行，从而实现子改父！
>
> 理解二：关于生命周期函数的延续
>
> - 组件第一次渲染
>   - 父 willMount -> 父 render
>   - 子 willMount -> 子 render -> 子 didMount
>   - 父 didMount
> - 组件更新
>   - 父 shouldUpdate -> 父 willUpdate -> 父 render
>   - 子 willReciveProps -> 子 shouldUpdate -> 子 willUpdate -> 子 render -> 子 didUpdate
>   - 父 didUpdate

#### 函数组件

![image-20240119204332402](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401192043727.png)

Vote.jsx

```jsx
import React, { useCallback, useState } from "react";
import './Vote.less';
import VoteMain from './VoteMain';
import VoteFooter from './VoteFooter';

const Vote = function Vote() {
    let [supNum, setSupNum] = useState(10);
    let [oppNum, setOppNum] = useState(0);
    const change = useCallback((type)=> {
        if(type === 'sup') {
            setSupNum(supNum + 1);
        }else {
            setOppNum(oppNum + 1);
        }
    }, [supNum, oppNum])
    return <div className="vote-box">
        <div className="header">
            <h2 className="title">React是很棒的前端框架</h2>
            <span className="num">{supNum + oppNum}</span>
        </div>
        <VoteMain supNum={supNum} oppNum = {oppNum} />
        <VoteFooter change={change} />
    </div>;
};

export default Vote;
```

VoteMain.jsx

```jsx
import React, { useMemo } from "react";
import {PropTypes} from 'prop-types'

const VoteMain = function VoteMain(props) {
    let {supNum, oppNum} = props;
    let ratio = '--'
    ratio = useMemo(() => {
        let ratio = '--',
        total = supNum + oppNum;
        if (total > 0) ratio = (supNum / total * 100).toFixed(2) + '%';
        return ratio;
    }, [supNum, oppNum])
    return <div className="main">
        <p>支持人数：{supNum}人</p>
        <p>反对人数：{oppNum}人</p>
        <p>支持比率：{ratio}</p>
    </div>;
};
// 规则属性校验
VoteMain.defaultProps = {
    supNum: 0,
    oppNum: 0
}
VoteMain.propTypes = {
    supNum: PropTypes.number,
    oppNum: PropTypes.number
}
export default VoteMain;
```

VoteFooter.jsx

```jsx
import React from "react";
import { Button } from 'antd';
import PropTypes from 'prop-types';

const VoteFooter = function VoteFooter(props) {
    let {change} = props
    return <div className="footer">
        <Button type="primary" onClick={change.bind(null, 'sup')}>支持</Button>
        <Button type="primary" onClick={change.bind(null, 'opp')} danger>反对</Button>
    </div>;
};
// 规则属性校验
VoteFooter.defaultProps = {}
VoteFooter.propTypes = {
    change: PropTypes.func.isRequired
}

export default VoteFooter;
```

![image-20240119212057789](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401192120974.png)

### 基于context上下文，实现祖先/后代(或平行)组件间的通信

![下载](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401252259137.png)

1.创建上下文对象

ThemeContext.js

```javascript
import React from "react";
const ThemeContext = React.createContext();
export default ThemeContext;
```

#### 类组件

![image-20240125231240954](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401252312156.png)

![image-20240125231929394](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401252319577.png)

![image-20240125232134183](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401252321362.png)

Vote.jsx

```jsx
import React from "react";
import './Vote.less';
import VoteMain from './VoteMain';
import VoteFooter from './VoteFooter';
import ThemeContext from "../../ThemeContext";

class Vote extends React.Component {
    state = {
        supNum: 10,
        oppNum: 0
    }
    // 设置为箭头函数：不论方法在哪执行的，方法中的this永远都是Vote父组件的实例
    change = (type) => {
        let { supNum, oppNum} = this.state
        if(type === 'sup') {
            this.setState({
                supNum: supNum + 1
            })
            return;
        }
        this.setState({oppNum: oppNum + 1})
    }
    render() {
        let {supNum, oppNum} = this.state;
        return <ThemeContext.Provider value={{
            supNum,
            oppNum,
            change: this.change
        }}>
            <div className="vote-box">
                <div className="header">
                    <h2 className="title">React是很棒的前端框架</h2>
                    <span className="num">{supNum + oppNum}</span>
                </div>
                <VoteMain/>
                <VoteFooter/>
            </div>
        </ThemeContext.Provider>;
    }
}

export default Vote;
```

VoteMain.jsx

```jsx
import React from "react";
import ThemeContext from "../../ThemeContext";

class VoteMain extends React.Component {
    render() {
        return <ThemeContext.Consumer>
            {
                context => {
                    let {supNum, oppNum} = context
                    return <div className="main">
                        <p>支持人数：{supNum}人</p>
                        <p>反对人数：{oppNum}人</p>
                        <p>支持比率：--</p>
                    </div>
                }
            }
        </ThemeContext.Consumer>
    }
}

export default VoteMain;
```

VoteFooter.jsx

```jsx
import React from "react";
import { Button } from 'antd';
import ThemeContext from "../../ThemeContext";

// class VoteFooter extends React.Component {
//     render() {
//         return <ThemeContext.Consumer>
//             {
//                 context => {
//                     let {change} = context
//                     return <div className="footer">
//                         <Button type="primary" onClick={change.bind(null, 'sup')}>支持</Button>
//                         <Button type="primary" onClick={change.bind(null, 'opp')} danger>反对</Button>
//                     </div>; 
//                 }
//             }
//         </ThemeContext.Consumer>
//     }
// }

// 方案二
class VoteFooter extends React.Component {
    static contextType = ThemeContext;
    render() {
        let {change} = this.context
        return <div className="footer">
            <Button type="primary" onClick={change.bind(null, 'sup')}>支持</Button>
            <Button type="primary" onClick={change.bind(null, 'opp')} danger>反对</Button>
        </div>; 
    }
}

export default VoteFooter;
```

#### 函数组件

Vote.jsx

```jsx
import React, { useCallback, useState } from "react";
import './Vote.less';
import VoteMain from './VoteMain';
import VoteFooter from './VoteFooter';
import ThemeContext from "../../ThemeContext";

const Vote = function Vote() {
    let [supNum, setSupNum] = useState(10);
    let [oppNum, setOppNum] = useState(0);
    const change = useCallback((type)=> {
        if(type === 'sup') {
            setSupNum(supNum + 1);
        }else {
            setOppNum(oppNum + 1);
        }
    }, [supNum, oppNum])
    return (
        <ThemeContext.Provider value={{
            supNum,
            oppNum,
            change
        }}>
            <div className="vote-box">
                <div className="header">
                    <h2 className="title">React是很棒的前端框架</h2>
                    <span className="num">{supNum + oppNum}</span>
                </div>
                <VoteMain supNum={supNum} oppNum = {oppNum} />
                <VoteFooter change={change} />
            </div>
        </ThemeContext.Provider>
    )
};

export default Vote;
```

VoteMain.jsx

```jsx
import React, { useContext, useMemo } from "react";
import ThemeContext from "../../ThemeContext";

const VoteMain = function VoteMain() {
    // 获取上下文中的信息
    let {supNum, oppNum} = useContext(ThemeContext);
    let ratio = useMemo(() => {
        let total = supNum + oppNum;
        return total > 0 ? (supNum / total * 100).toFixed(2) + '%' : '--';
    }, [supNum, oppNum]);
    return <div className="main">
        <p>支持人数：{supNum}人</p>
        <p>反对人数：{oppNum}人</p>
        <p>支持比率：{ratio}</p>
    </div>;
};
export default VoteMain;
```

VoteFooter.jsx

```jsx
import React, { useContext } from "react";
import { Button } from 'antd';
import ThemeContext from "../../ThemeContext";

const VoteFooter = function VoteFooter() {
    let {change} = useContext(ThemeContext);
    return <div className="footer">
        <Button type="primary" onClick={change.bind(null, 'sup')}>支持</Button>
        <Button type="primary" onClick={change.bind(null, 'opp')} danger>反对</Button>
    </div>;
};
export default VoteFooter;
```

> 真实项目中
>
> - 父子通信（或具备相同父亲的兄弟组件）：我们一般都是基于props属性实现
> - 其他组件之间的通信：我们都是基于 redux / react-redux 或者 mobx 去实现

## React样式的处理方案

在vue开发中，我们可以基于`scoped`为组件设置样式私有化！

```vue
<style lang="less" scoped>
.banner-box {
  box-sizing: border-box;
  height: 375px;
  background: #eee;
  overflow: hidden;
}
:deep(.van-swipe__indicators) {
    left: auto;
    right: 20px;
    transform: none;
}
</style>
```

![下载](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401262340036.png)

但是react项目中并没有类似于这样的机制！如果我们想保证“团队协作开发”中，各组件间的样式不冲突，我们则需要基于特定的方案进行处理！

### 内联样式

内联样式就是在JSX元素中，直接定义行内的样式

```jsx
// 调用组件的时候 <Demo color="red" />
import React from 'react';
const Demo = function Demo(props) {
    const titleSty = {
        color: props.color,
        fontSize: '16px'
    };
    const boxSty = {
        width: '300px',
        height: '200px'
    };
    return <div style={boxSty}>
        <h1 style={titleSty}>1111</h1>
        <h2 style={{ ...titleSty, fontSize: '14px' }}>2222</h2>
    </div>;
};
export default Demo;
```

编译后的内容

```html
<div style="width: 300px; height: 200px;">
    <h1 style="color: red; font-size: 16px;">珠峰培训</h1>
    <h2 style="color: red; font-size: 14px;">珠峰培训</h2>
</div>
```

内联样式的优点：

- **使用简单：** 简单的以组件为中心来实现样式的添加
- **扩展方便：** 通过使用对象进行样式设置，可以方便的扩展对象来扩展样式
- **避免冲突：** 最终都编译为元素的行内样式，不存在样式冲突的问题

在大型项目中，内联样式可能并不是一个很好的选择，因为内联样式还是有局限性的：

- **不能使用伪类：** 这意味着 :hover、:focus、:actived、:visited 等都将无法使用
- **不能使用媒体查询：** 媒体查询相关的属性不能使用
- **减低代码可读性：** 如果使用很多的样式，代码的可读性将大大降低
- **没有代码提示：** 当使用对象来定义样式时，是没有代码提示的

### 使用CSS样式表

CSS样式表应该是我们最常用的定义样式的方式！但多人协作开发中，很容易导致组件间的样式类名冲突，从而导致样式冲突；所以此时需要我们 `人为有意识的` 避免冲突！

- 保证组件最外层样式类名的唯一性，例如：`路径名称 + 组件名称` 作为样式类名
- 基于 less、sass、stylus 等css预编译语言的`嵌套功能`，保证组件后代元素的样式，都嵌入在外层样式类中！！

Demo.less

```less
.personal-box {
    width: 300px;
    height: 200px;
    .title {
        color: red;
        font-size: 16px;
    }
    .sub-title {
        .title;
        font-size: 14px;
    }
}
```

Demo.jsx

```jsx
import React from 'react';
import './Demo.less';
const Demo = function Demo(props) {
    return <div className='personal-box'>
        <h1 className='title'>111</h1>
        <h2 className='sub-title'>222</h2>
    </div>;
};
export default Demo;
```

CSS样式表的优点：

- **结构样式分离：** 实现了样式和JavaScript的分离
- **使用CSS所有功能：** 此方法允许我们使用CSS的任何语法，包括伪类、媒体查询等
- **使用缓存：** 可对样式文件进行强缓存或协商缓存
- **易编写**：CSS样式表在书写时会有代码提示

当然，CSS样式表也是有缺点的：

- **产生冲突：** CSS选择器都具有相同的全局作用域，很容易造成样式冲突
- **性能低：** 预编译语言的嵌套，可能带来的就是超长的`选择器前缀`，性能低！
- **没有真正的动态样式：** 在CSS表中难以实现动态设置样式

### CSS Modules

CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效；产生局部作用域的唯一方法，就是使用一个独一无二的class名字；这就是 CSS Modules 的做法！

第一步：创建 xxx.module.css

```css
.personal {
  width: 300px;
  height: 200px;
}
.personal span {
  color: green;
}
.title {
  color: red;
  font-size: 16px;
}
.subTitle {
  color: red;
  font-size: 14px;
}
```

第二步：导入样式文件 & 调用

```jsx
import React from 'react';
import sty from './css/demo.module.css';
const Demo = function Demo() {
    return <div className={sty.personal}>
        <h1 className={sty.title}>111</h1>
        <h2 className={sty.subTitle}>222</h2>
        <span>文字</span>
    </div>;
};
export default Demo;
```

![image-20240127231139428](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401272311525.png)

编译后的效果

```html
// 结构
<div class="demo_personal__dlx2V">
    <h1 class="demo_title__tN+WF">111</h1>
    <h2 class="demo_subTitle__rR4WF">222</h2>
    <span>文字</span>
</div>

// 样式
.demo_personal__dlx2V {
    height: 200px;
    width: 300px
}
.demo_personal__dlx2V span {
    color: green
}
.demo_title__tN\+WF {
    color: red;
    font-size: 16px
}
.demo_subTitle__rR4WF {
    color: red;
    font-size: 14px
}
```

react 脚手架中对 CSS Modules 的配置

```js
// react-dev-utils/getCSSModuleLocalIdent.js
const loaderUtils = require('loader-utils');
const path = require('path');
module.exports = function getLocalIdent(
  context,
  localIdentName,
  localName,
  options
) {
  // Use the filename or folder name, based on some uses the index.js / index.module.(css|scss|sass) project style
  const fileNameOrFolder = context.resourcePath.match(
    /index\.module\.(css|scss|sass)$/
  )
    ? '[folder]'
    : '[name]';
  // Create a hash based on a the file location and class name. Will be unique across a project, and close to globally unique.
  const hash = loaderUtils.getHashDigest(
    path.posix.relative(context.rootContext, context.resourcePath) + localName,
    'md5',
    'base64',
    5
  );
  // Use loaderUtils to find the file or folder name
  const className = loaderUtils.interpolateName(
    context,
    fileNameOrFolder + '_' + localName + '__' + hash,
    options
  );
  // Remove the .module that appears in every classname when based on the file and replace all "." with "_".
  return className.replace('.module_', '_').replace(/\./g, '_');
};
```

#### 全局作用域

CSS Modules 允许使用 :global(.className) 的语法，声明一个全局规则。凡是这样声明的class，都不会被编译成哈希字符串。

```css
// xxx.module.css
:global(.personal) {
    width: 300px;
    height: 200px;
}

// xxx.jsx
const Demo = function Demo() {
    return <div className='personal'>
        ...
    </div>;
};
```

#### class继承/组合

在 CSS Modules 中，一个选择器可以继承另一个选择器的规则，这称为”组合”

```css
// xxx.module.css
.title {
    color: red;
    font-size: 16px;
}
.subTitle {
    composes: title;
    font-size: 14px;
}

<h1 class="demo_title__tN+WF">111</h1>
<h2 class="demo_subTitle__rR4WF">222</h2>


// 组件还是正常的调用，但是编译后的结果
<h1 class="demo_title__tN+WF">111</h1>
<h2 class="demo_subTitle__rR4WF demo_title__tN+WF">222</h2>
```

![image-20240127232112585](https://trpora-1300527744.cos.ap-chongqing.myqcloud.com/img/202401272321706.png)