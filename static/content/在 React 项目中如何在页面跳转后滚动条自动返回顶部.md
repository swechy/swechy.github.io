
## 问题说明
在 React Router v6 中，没有像 Vue Router 那样直接的 “路由守卫” 概念，所以我们可以通过一些替代方案来实现类似的功能。

比如我们使用 `react-router-dom` 的 `useNavigate` 或者 `Link` 来实现路由跳转时，跳转后的页面滚动条是不会返回顶部的，而是停留在当前浏览位置，所以我们可以手动实现一个监控，来使所有的 `navigate` 跳转后页面滚动条都返回顶部。

而在 React 中，要在每次 `navigate` 操作后将滚动条回到顶端，可以使用 `react-router-dom` 提供的 `useLocation` 钩子和 `useEffect` 钩子来实现。`useLocation` 用于监听路由的变化，而 `useEffect` 可以在路由变化时执行滚动操作。

```js
import React, { useEffect } from'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from'react-router-dom';

// 模拟页面组件
const Home = () => <div>Home Page</div>;
const About = () => <div>About Page</div>;

// 自定义路由组件，用于处理滚动条回到顶端
const ScrollToTop = ({ children }) => {
  const location = useLocation();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [location]);

  return children;
};

const App = () => {
  return (
    <Router>
      <ScrollToTop>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </ScrollToTop>
    </Router>
  );
};

export default App;
```

## 代码说明
1. **导入必要的库**：从 `react-router-dom` 导入 `BrowserRouter as Router`、`Routes`、`Route` 和 `useLocation`，从 `react` 导入 `useEffect`。
2. **创建页面组件**：这里创建了 `Home` 和 `About` 两个简单的页面组件。
3. **创建 `ScrollToTop` 组件**：
    - 使用 `useLocation` 钩子获取当前的路由位置信息。
    - 使用 `useEffect` 钩子，依赖项为 `location`。这意味着每当路由位置发生变化（即 `navigate` 操作后），`useEffect` 中的回调函数会被执行。
    - 在回调函数中，使用 `window.scrollTo(0, 0)` 将滚动条滚动到页面顶端。
    - 最后返回 `children`，这样可以将包裹在 `ScrollToTop` 组件内的内容正常渲染。
4. **在 `App` 组件中使用 `ScrollToTop`**：将 `ScrollToTop` 组件包裹在 `Routes` 组件外部，这样每次路由切换时，都会触发 `ScrollToTop` 组件中的 `useEffect`，从而实现滚动条回到顶端的效果。

如果你使用的是 React Router v6 以上版本，代码结构基本相同，但在导入和使用上可能会有一些细微差异。例如，`BrowserRouter` 被重命名为 `createBrowserRouter` 和 `RouterProvider` 的组合方式：

```js
import React from'react';
import { createBrowserRouter, RouterProvider } from'react-router-dom';

// 模拟页面组件
const Home = () => <div>Home Page</div>;
const About = () => <div>About Page</div>;

// 自定义路由组件，用于处理滚动条回到顶端
const ScrollToTop = ({ children }) => {
  const { location } = useLocation();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [location]);

  return children;
};

const router = createBrowserRouter([
  {
    path: '/',
    element: <ScrollToTop><Home /></ScrollToTop>
  },
  {
    path: '/about',
    element: <ScrollToTop><About /></ScrollToTop>
  }
]);

const App = () => {
  return <RouterProvider router={router} />;
};

export default App;
```

在这个版本中，通过 `createBrowserRouter` 创建路由配置，并使用 `RouterProvider` 来渲染路由。在每个路由的 `element` 中包裹 `ScrollToTop` 组件，以实现滚动条回到顶端的功能。  