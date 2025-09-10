## Storybook 简介
Storybook 是一个用于开发 UI 组件的工具，它允许开发者在隔离的环境中创建和测试组件。它的核心思想是将组件的开发和应用的整体逻辑分离，使得开发者可以专注于单个组件的设计、开发和调试。Storybook 支持多种前端框架，如 React、Vue、Angular 等，这里我们主要关注它在 React 项目中的使用。

Storybook 具有以下优点：
- **组件隔离开发**：在 Storybook 中，每个组件都可以在独立的环境中进行开发和测试，不受其他组件和应用逻辑的干扰。这有助于开发者更专注于组件本身的功能和样式。
- **可视化展示**：Storybook 提供了一个直观的界面，以可视化的方式展示组件的不同状态和交互效果。开发者可以实时查看组件在不同输入和操作下的表现。
- **文档生成**：它可以自动生成组件的文档，包括组件的描述、使用方法、API 等信息。这对于团队协作和组件的复用非常有帮助。
- **支持多种测试**：Storybook 与多种测试框架集成，如 Jest 和 React Testing Library，方便开发者对组件进行单元测试和交互测试。

## 在 React 项目中安装 Storybook
在 React 项目的根目录下，使用以下命令安装 Storybook：

```bash
npx sb init
```

这个命令会自动安装 Storybook 及其相关依赖，并为项目生成初始的配置文件和目录结构。安装完成后，项目目录中会新增一个 `.storybook` 目录，用于存放 Storybook 的配置文件，同时还会在 `src/stories` 目录下生成一些示例故事文件。

## 创建 Storybook 故事
Storybook 中的“故事”是指展示组件不同状态的示例。每个故事都是一个函数，返回要展示的组件。

例如，我们有一个简单的 `Button` 组件：

```js
import React from'react';

const Button = ({ label, onClick }) => {
  return <button onClick={onClick}>{label}</button>;
};

export default Button;
```

接下来，我们为 `Button` 组件创建 Storybook 故事。在 `src/stories` 目录下创建一个 `Button.stories.js` 文件：

```js
import React from'react';
import Button from '../components/Button';
import { storiesOf } from '@storybook/react';

storiesOf('Button', module)
 .add('default', () => <Button label="Click me" onClick={() => {}} />)
 .add('disabled', () => <Button label="Disabled" onClick={() => {}} disabled />);
```

在上述代码中：
- `storiesOf` 函数用于创建一个故事集合，第一个参数是故事集合的名称（这里是 `Button`），第二个参数是 `module`，用于指定当前模块。
- `.add` 方法用于向故事集合中添加具体的故事。第一个参数是故事的名称（如 `default` 和 `disabled`），第二个参数是一个函数，返回要展示的组件实例。

## 运行 Storybook
在项目根目录下，运行以下命令启动 Storybook：

```bash
npm run storybook
```

或者如果安装时使用了 yarn：

```bash
yarn storybook
```

启动后，Storybook 会在浏览器中打开一个界面，展示我们创建的所有故事。在这里，我们可以方便地查看 `Button` 组件的不同状态，如默认状态和禁用状态。

## 配置 Storybook
## 主题定制
Storybook 支持主题定制，我们可以根据项目需求调整界面的颜色、字体等样式。在 `.storybook` 目录下创建一个 `theme.js` 文件：

```js
import { create } from '@storybook/theming';

export default create({
  base: 'light',
  brandTitle: 'My React Components',
  brandUrl: 'https://example.com',
  brandImage: 'https://example.com/logo.png',
  brandTarget: '_blank'
});
```

然后，在 `.storybook/main.js` 文件中引入这个主题：

```js
module.exports = {
  //...其他配置
  core: {
    builder: 'webpack5'
  },
  stories: ['../src/stories/*.stories.mdx', '../src/stories/*.stories.@(js|jsx|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials'
  ],
  webpackFinal: async (config) => {
    // 在这里进行 webpack 配置的修改
    return config;
  },
  theme: require('./theme.js')
};
```

## 添加插件
Storybook 有丰富的插件生态系统，可以通过添加插件来扩展其功能。例如，要添加 `@storybook/addon-controls` 插件，以便在 Storybook 中动态控制组件的属性：

首先，安装插件：

```bash
npm install @storybook/addon-controls --save-dev
```

然后，在 `.storybook/main.js` 文件中添加插件：

```js
module.exports = {
  //...其他配置
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-controls'
  ]
};
```

修改 `Button.stories.js` 文件，使用 `addon-controls`：

```js
import React from'react';
import Button from '../components/Button';
import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import { withKnobs, text, boolean } from '@storybook/addon-knobs';

storiesOf('Button', module)
 .addDecorator(withKnobs)
 .add('default', () => {
      const label = text('Label', 'Click me');
      const disabled = boolean('Disabled', false);
      return <Button label={label} onClick={action('button clicked')} disabled={disabled} />;
    });
```

现在，在 Storybook 界面中，我们可以通过滑动条、输入框等控件动态修改 `Button` 组件的属性。

## 测试集成
Storybook 可以与测试框架集成，方便对组件进行测试。例如，与 Jest 和 React Testing Library 集成：

首先，安装依赖：

```bash
npm install --save-dev @storybook/addon-jest jest @testing-library/react
```

在 `.storybook/main.js` 文件中添加 `@storybook/addon-jest` 插件：

```js
module.exports = {
  //...其他配置
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-controls',
    '@storybook/addon-jest'
  ]
};
```

为 `Button` 组件编写测试用例，在 `src/components` 目录下创建 `Button.test.js` 文件：

```js
import React from'react';
import { render, fireEvent } from '@testing-library/react';
import Button from './Button';

test('Button calls onClick when clicked', () => {
  const handleClick = jest.fn();
  const { getByText } = render(<Button label="Test Button" onClick={handleClick} />);
  fireEvent.click(getByText('Test Button'));
  expect(handleClick).toHaveBeenCalled();
});
```

运行测试：

```bash
npm test
```

测试结果会在 Storybook 界面中显示，方便我们查看组件的测试状态。

## 总结
Storybook 是一款功能强大的工具，为 React 组件的开发、展示和测试提供了一站式解决方案。通过隔离组件开发、可视化展示、文档生成和测试集成等功能，它能够提高开发效率，增强组件的质量和可维护性。在实际项目中，合理运用 Storybook 可以让我们的 React 开发流程更加顺畅和高效。  