## Axios 简介
Axios 是一个可以在浏览器和 Node.js 中使用的 HTTP 客户端库。它基于 Promise 设计，这使得处理异步请求变得更加简单和直观。Axios 具有以下特点：
- **支持多种请求方式**：如 GET、POST、PUT、DELETE 等常见的 HTTP 请求方法。
- **浏览器和 Node.js 环境兼容**：可以在 React 应用的前端代码中使用，也可以在 Node.js 服务器端代码中使用。
- **自动转换 JSON 数据**：在发送和接收数据时，Axios 会自动对 JSON 数据进行序列化和反序列化。
- **拦截器功能**：可以在请求发送前和响应接收后进行拦截和处理，例如添加请求头、处理错误等。

## 安装 Axios
在 React 项目的根目录下，使用 npm 或 yarn 安装 Axios：

```bash
# 使用 npm 安装
npm install axios

# 使用 yarn 安装
yarn add axios
```

## 基本的 GET 请求
## 简单的 GET 请求
在 React 组件中发起一个简单的 GET 请求，获取数据。

```js
import React, { useEffect, useState } from'react';
import axios from 'axios';

const GetDataComponent = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await axios.get('https://example.com/api/data');
        setData(response.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };
    fetchData();
  }, []);

  if (loading) {
    return <p>Loading...</p>;
  }
  if (error) {
    return <p>Error: {error.message}</p>;
  }
  return (
    <div>
      <h1>Data from API</h1>
      {data && <pre>{JSON.stringify(data, null, 2)}</pre>}
    </div>
  );
};

export default GetDataComponent;
```

在上述代码中：
- 使用 `useState` Hook 来管理数据（`data`）、加载状态（`loading`）和错误信息（`error`）。
- `useEffect` Hook 在组件挂载时执行一次 `fetchData` 函数。
- `fetchData` 函数使用 `axios.get` 方法发起 GET 请求，获取数据。如果请求成功，将数据存储在 `data` 状态中；如果发生错误，将错误信息存储在 `error` 状态中。

## 带参数的 GET 请求
有时候需要在 GET 请求中传递参数。例如，从服务器获取特定 ID 的数据。

```js
import React, { useEffect, useState } from'react';
import axios from 'axios';

const GetDataWithParamsComponent = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const itemId = 1; // 假设要获取的 item ID

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await axios.get(`https://example.com/api/data/${itemId}`);
        setData(response.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };
    fetchData();
  }, []);

  if (loading) {
    return <p>Loading...</p>;
  }
  if (error) {
    return <p>Error: {error.message}</p>;
  }
  return (
    <div>
      <h1>Data with ID {itemId}</h1>
      {data && <pre>{JSON.stringify(data, null, 2)}</pre>}
    </div>
  );
};

export default GetDataWithParamsComponent;
```

或者通过查询参数传递多个值：

```js
import React, { useEffect, useState } from'react';
import axios from 'axios';

const GetDataWithQueryParamsComponent = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const params = {
    category: 'electronics',
    page: 1
  };

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await axios.get('https://example.com/api/products', { params });
        setData(response.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };
    fetchData();
  }, []);

  if (loading) {
    return <p>Loading...</p>;
  }
  if (error) {
    return <p>Error: {error.message}</p>;
  }
  return (
    <div>
      <h1>Products Data</h1>
      {data && <pre>{JSON.stringify(data, null, 2)}</pre>}
    </div>
  );
};

export default GetDataWithQueryParamsComponent;
```

## 基本的 POST 请求
使用 Axios 发送 POST 请求，通常用于向服务器提交数据。

```js
import React, { useState } from'react';
import axios from 'axios';

const PostDataComponent = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: ''
  });
  const [responseData, setResponseData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const handleChange = (e) => {
    setFormData({
    ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    setLoading(true);
    try {
      const response = await axios.post('https://example.com/api/users', formData);
      setResponseData(response.data);
    } catch (error) {
      setError(error);
    } finally {
      setLoading(false);
    }
  };

  if (loading) {
    return <p>Loading...</p>;
  }
  if (error) {
    return <p>Error: {error.message}</p>;
  }
  return (
    <div>
      <h1>Post Data Example</h1>
      <form onSubmit={handleSubmit}>
        <label>
          Name:
          <input type="text" name="name" value={formData.name} onChange={handleChange} />
        </label>
        <br />
        <label>
          Email:
          <input type="email" name="email" value={formData.email} onChange={handleChange} />
        </label>
        <br />
        <button type="submit">Submit</button>
      </form>
      {responseData && <pre>{JSON.stringify(responseData, null, 2)}</pre>}
    </div>
  );
};

export default PostDataComponent;
```

在上述代码中，用户在表单中输入数据，点击提交按钮后，使用 `axios.post` 方法将数据发送到服务器。如果请求成功，将服务器返回的数据存储在 `responseData` 状态中。

## 拦截器的使用
Axios 的拦截器可以在请求发送前和响应接收后进行拦截和处理。

## 请求拦截器
例如，在每个请求中添加一个认证令牌：

```js
import axios from 'axios';

axios.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);
```

在上述代码中，`axios.interceptors.request.use` 用于添加请求拦截器。在请求发送前，检查本地存储中是否有令牌，如果有，则将其添加到请求头的 `Authorization` 字段中。

## 响应拦截器
例如，统一处理所有响应中的错误：

```js
import axios from 'axios';

axios.interceptors.response.use(
  (response) => {
    return response;
  },
  (error) => {
    if (error.response) {
      // 处理 401 未授权错误
      if (error.response.status === 401) {
        // 可以在这里进行重定向到登录页面等操作
        console.log('Unauthorized, redirect to login page');
      }
    }
    return Promise.reject(error);
  }
);
```

在上述代码中，`axios.interceptors.response.use` 用于添加响应拦截器。如果响应状态码为 401（未授权），可以进行相应的处理，如重定向到登录页面。

## 将Axios封装成一个函数

```js
/**
 * 网络请求配置
 */
import axios from "axios";

axios.defaults.timeout = 100000;
axios.defaults.baseURL = window.location.protocol + "//" + window.location.host;

/**
 * http request 拦截器
 */
axios.interceptors.request.use(
  (config) => {
    config.data = JSON.stringify(config.data);
    config.headers = {
      "Content-Type": "application/json",
    };
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

/**
 * http response 拦截器
 */
axios.interceptors.response.use(
  (response) => {
    if (response.data.errCode === 2) {
      console.log("过期");
    }
    return response;
  },
  (error) => {
    console.log("请求出错：", error);
  }
);

/**
 * 封装get方法
 * @param url  请求url
 * @param params  请求参数
 * @returns {Promise}
 */
export function get(url, params = {}) {
  return new Promise((resolve, reject) => {
    axios
      .get(url, {
        params: params,
      })
      .then((response) => {
        landing(url, params, response.data);
        resolve(response.data);
      })
      .catch((error) => {
        reject(error);
      });
  });
}

/**
 * 封装post请求
 * @param url
 * @param data
 * @returns {Promise}
 */

export function post(url, data) {
  return new Promise((resolve, reject) => {
    axios.post(url, data).then(
      (response) => {
        //关闭进度条
        resolve(response.data);
      },
      (err) => {
        reject(err);
      }
    );
  });
}

/**
 * 封装patch请求
 * @param url
 * @param data
 * @returns {Promise}
 */
export function patch(url, data = {}) {
  return new Promise((resolve, reject) => {
    axios.patch(url, data).then(
      (response) => {
        resolve(response.data);
      },
      (err) => {
        msag(err);
        reject(err);
      }
    );
  });
}

/**
 * 封装put请求
 * @param url
 * @param data
 * @returns {Promise}
 */

export function put(url, data = {}) {
  return new Promise((resolve, reject) => {
    axios.put(url, data).then(
      (response) => {
        resolve(response.data);
      },
      (err) => {
        msag(err);
        reject(err);
      }
    );
  });
}

//统一接口处理，返回数据
export default function (fecth, url, param) {
  let _data = "";
  return new Promise((resolve, reject) => {
    switch (fecth) {
      case "get":
        console.log("begin a get request,and url:", url);
        get(url, param)
          .then(function (response) {
            resolve(response);
          })
          .catch(function (error) {
            console.log("get request GET failed.", error);
            reject(error);
          });
        break;
      case "post":
        post(url, param)
          .then(function (response) {
            resolve(response);
          })
          .catch(function (error) {
            console.log("get request POST failed.", error);
            reject(error);
          });
        break;
      default:
        break;
    }
  });
}

//失败提示
function msag(err) {
  if (err && err.response) {
    switch (err.response.status) {
      case 400:
        alert(err.response.data.error.details);
        break;
      case 401:
        alert("未授权，请登录");
        break;

      case 403:
        alert("拒绝访问");
        break;

      case 404:
        alert("请求地址出错");
        break;

      case 408:
        alert("请求超时");
        break;

      case 500:
        alert("服务器内部错误");
        break;

      case 501:
        alert("服务未实现");
        break;

      case 502:
        alert("网关错误");
        break;

      case 503:
        alert("服务不可用");
        break;

      case 504:
        alert("网关超时");
        break;

      case 505:
        alert("HTTP版本不受支持");
        break;
      default:
    }
  }
}

/**
 * 查看返回的数据
 * @param url
 * @param params
 * @param data
 */
function landing(url, params, data) {
  if (data.code === -1) {
  }
}
```

## 总结
Axios 为 React 项目提供了一种简单、高效的网络请求解决方案。通过基本的 GET 和 POST 请求，以及灵活的拦截器功能，开发者可以轻松地与后端服务器进行数据交互，并且能够对请求和响应进行统一的处理和管理。在实际项目中，合理运用 Axios 可以提高应用的性能和用户体验。希望本文的介绍能帮助你在 React 项目中更好地使用 Axios 进行网络请求。  