## 一、React 脚手架简介
React 脚手架，即 Create React App，是 Facebook 官方提供的一个用于快速创建 React 项目的工具。它基于 Webpack、ES6 和 ESLint 等技术，提供了开箱即用的配置，让你无需手动配置复杂的开发环境，即可快速上手 React 开发。

## 二、安装 React 脚手架
首先，你需要全局安装 Create React App。打开你的命令行工具（如 cmd、Terminal 或 PowerShell），输入以下命令：

```bash
npm install -g create-react-app
```
或者，你也可以使用 Yarn 来安装：

```bash
yarn global add create-react-app
```
安装完成后，你可以通过 create-react-app 命令来创建新的 React 项目。

## 三、创建 React 项目
选择目录：使用 cd 命令切换到你想创建项目的目录。
创建项目：输入以下命令来创建一个新的 React 项目：
```bash
create-react-app my-react-app
```
其中 my-react-app 是你的项目名称，你可以根据需要替换为其他名称。

进入项目目录：使用 cd 命令进入你刚创建的项目目录：
```bash
cd my-react-app
```
启动项目：输入以下命令来启动你的 React 项目：
```bash
npm start
```
或者，如果你使用的是 Yarn：

```bash
yarn start
```
启动成功后，你的浏览器会自动打开一个新的标签页，显示你的 React 应用。默认情况下，应用会运行在 http://localhost:3000。

## 四、项目目录结构
启动项目后，你可以看到项目目录下生成了一系列文件和文件夹。以下是主要的文件和文件夹及其作用：

- node_modules：存放项目依赖的模块。
- public：存放静态资源文件，如 index.html、favicon.ico 等。
- src：存放源码文件，包括组件、样式、测试等。
- App.css：App 组件的样式文件。
- App.js：App 组件的 JavaScript 文件。
- App.test.js：App 组件的测试文件。
- index.css：全局样式文件。
- index.js：应用的入口文件。
- logo.svg：应用的 Logo 图标。
- reportWebVitals.js：用于性能分析的文件。
- setupTests.js：用于配置测试环境的文件。

## 五、开始开发
现在，你可以开始编写你的 React 应用了。以下是一个简单的示例，教你如何修改 App.js 文件来显示一个 Hello World 组件：

打开 src/App.js 文件。
修改文件内容如下：
```js
import React from 'react';
import './App.css';
 
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <p>Hello, World!</p>
      </header>
    </div>
  );
}
 
export default App;
```
保存文件后，你的浏览器会自动刷新，并显示修改后的内容。