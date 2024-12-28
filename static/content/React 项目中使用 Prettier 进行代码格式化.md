## Prettier 简介
Prettier 是一个“有态度的代码格式化工具”，它以一种固定的规则来格式化代码，而不考虑开发者个人的偏好。Prettier 支持多种编程语言，包括 JavaScript、JSX、CSS、HTML 等，非常适合用于 React 项目。它的主要特点包括：
- **统一代码风格**：无论团队成员的个人代码风格如何，Prettier 都能将代码格式化为一致的风格，使得代码库看起来更加整洁和规范。
- **自动格式化**：可以通过命令行工具或集成到开发环境中，自动对代码进行格式化，节省手动调整代码格式的时间。
- **支持多种编辑器**：Prettier 可以与常见的代码编辑器（如 Visual Studio Code、WebStorm 等）集成，在编写代码时实时进行格式化。

## 在 React 项目中安装 Prettier
在 React 项目的根目录下，使用 npm 或 yarn 安装 Prettier：

```bash
# 使用 npm 安装
npm install prettier --save-dev

# 使用 yarn 安装
yarn add prettier --save-dev
```

如果需要在 React 项目中处理 JSX 文件，还需要安装 `prettier-plugin - babel`，它可以让 Prettier 正确处理 Babel 语法：

```bash
# 使用 npm 安装
npm install prettier-plugin-babel --save-dev

# 使用 yarn 安装
yarn add prettier-plugin-babel --save-dev
```

## 配置 Prettier
在项目根目录下创建一个 `.prettierrc` 文件（也可以使用 `.prettierrc.json`、`.prettierrc.js` 等格式），用于配置 Prettier 的规则。以下是一个基本的配置示例：

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5"
}
```

上述配置的含义如下：
- `semi`：是否在语句末尾添加分号，`true` 表示添加，`false` 表示不添加。
- `singleQuote`：是否使用单引号，`true` 表示使用单引号，`false` 表示使用双引号。
- `trailingComma`：在对象和数组的最后一个元素后面是否添加逗号，`es5` 表示遵循 ES5 规范添加逗号。

你还可以根据项目的具体需求，在配置文件中添加更多的规则。例如：

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "printWidth": 80, // 每行代码的最大宽度
  "tabWidth": 2, // 一个制表符的空格数
  "bracketSpacing": true, // 在对象字面量的括号内是否添加空格
  "jsxBracketSameLine": false // JSX 标签的 > 是否另起一行
}
```

## 在项目中使用 Prettier
## 命令行格式化
在项目根目录下，运行以下命令对项目中的代码进行格式化：

```bash
npx prettier --write src
```

上述命令会对 `src` 目录下的所有文件进行格式化，并覆盖原文件。如果只想检查代码是否符合 Prettier 格式，而不进行修改，可以使用 `--check` 选项：

```bash
npx prettier --check src
```

该命令会输出不符合格式的文件列表，但不会修改文件内容。

## 集成到开发工具
## Visual Studio Code
在 Visual Studio Code 中，安装 `Prettier - Code formatter` 扩展。安装完成后，在编辑器中打开一个 React 文件，按下 `Ctrl + Shift + I`（Windows）或 `Command + Shift + I`（Mac）组合键，即可对当前文件进行格式化。

你还可以通过配置文件，让 Visual Studio Code 在保存文件时自动进行格式化。在 `settings.json` 文件中添加以下配置：

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true
}
```

## WebStorm
在 WebStorm 中，打开 `Settings`（或 `Preferences`），找到 `Languages & Frameworks` -> `JavaScript` -> `Prettier`，勾选 `Enable` 启用 Prettier。然后可以在 `Prettier` 配置页面中指定 Prettier 的路径和配置文件。

在 WebStorm 中，你可以通过 `Code` -> `Reformat Code` 菜单选项对当前文件或选中的代码进行格式化。

## 集成到 Git 钩子
为了确保提交到代码库的代码都是经过格式化的，可以将 Prettier 集成到 Git 钩子中。在项目根目录下，创建一个 `.husky` 目录（如果没有的话），然后在 `.husky` 目录下创建一个 `pre - commit` 文件：

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx prettier --check src
```

上述脚本会在每次提交代码前，检查 `src` 目录下的代码是否符合 Prettier 格式。如果有不符合的文件，提交将会失败。你可以根据需要修改检查的目录和文件范围。

## 与 ESLint 结合使用
虽然 Prettier 主要用于代码格式化，但它并不能完全替代 ESLint。ESLint 更侧重于检查代码中的语法错误和潜在的逻辑问题，而 Prettier 则专注于代码的外观格式。因此，在 React 项目中，通常会将两者结合使用。

首先，安装 `eslint - plugin - prettier` 和 `eslint - config - prettier`：

```bash
# 使用 npm 安装
npm install eslint-plugin-prettier eslint-config-prettier --save-dev

# 使用 yarn 安装
yarn add eslint-plugin-prettier eslint-config-prettier --save-dev
```

然后，在 `.eslintrc.js` 文件中进行配置：

```javascript
module.exports = {
  env: {
    browser: true,
    es2021: true
  },
  extends: [
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'plugin:prettier/recommended' // 启用 Prettier 作为 ESLint 的规则
  ],
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 'latest',
    sourceType:'module'
  },
  plugins: [
  'react',
  'react-hooks',
    'prettier'
  ],
  rules: {
    // 自定义规则
  }
};
```

`eslint - plugin - prettier` 会将 Prettier 的格式化结果作为 ESLint 的一条规则来检查，如果代码不符合 Prettier 格式，ESLint 会给出相应的错误提示。`eslint - config - prettier` 则会关闭 ESLint 中与 Prettier 冲突的规则，避免重复检查。

## 总结
在 React 项目中使用 Prettier 进行代码格式化，可以显著提高代码的可读性和一致性，促进团队协作。通过合理配置和集成到开发工具、Git 钩子中，以及与 ESLint 结合使用，可以打造一个高效、规范的开发环境。希望本文的介绍能够帮助你在 React 项目中顺利使用 Prettier，提升代码质量。  