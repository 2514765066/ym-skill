---
name: ym-code-style
description: 编写、修改或审查 TypeScript、JavaScript 及通用代码时应用个人代码风格，包括中文回复、函数式编程、命名、函数实现与交付检查。
---

# 通用代码规范

## 编码与命名

- 优先使用 TypeScript；无法使用时使用现代 JavaScript。
- 优先函数式编程：保持函数纯粹、数据不可变，使用 `map`、`filter`、`reduce` 等方式转换数据。
- 变量和函数使用小驼峰命名；文件和目录使用短横线命名。
- 使用表达业务语义的函数名；事件处理函数以 `handle` 开头，例如 `handleSubmitOrder`。
- 组件名使用帕斯卡命名法，组件文件使用短横线命名。

## 函数要求

- 禁止使用 `function` 关键字；普通函数和回调均使用箭头函数。
- 为箭头函数使用代码块，并显式写出 `return`。
- 需要定义类时使用 `class`，不要用函数模拟类。
- 为所有函数添加简短中文注释说明职责。
- 优先使用 `const`，避免使用 `let`；禁止使用 `var`。
- 将错误处理、无效输入和退出条件放在函数前部，使用早返回减少嵌套。

## 复杂声明注释

- 为复杂对象、数据推导和状态声明添加简短中文注释，说明声明的业务含义或判定目的。
- 对包含多行表达式、深层属性访问、可选链、空值回退或多个逻辑条件的声明，必须添加中文注释。
- 简单字面量、语义明确的直接赋值无需添加注释。

```ts
/** 优先使用显式宽度，缺省时回退到文本节点默认宽度 */
const editorWidth =
  explicitNodeSize.width ?? textNodeConfig.defaultSize.width;

/** 优先使用显式高度，缺省时回退到文本节点默认高度 */
const editorHeight =
  explicitNodeSize.height ?? textNodeConfig.defaultSize.height;

/** 标识当前文本节点是否处于编辑状态 */
const isEditing = data.ui?.isTextEditing === true;

/** 标识节点是否拥有静态文本或正在流式输出的 Markdown 内容 */
const hasDocument =
  data.textContent !== undefined ||
  data.ui?.streamingMarkdown?.content !== undefined;

/** 标识浮动 AI 面板是否应显示 */
const isFloatingAiVisible = data.ui?.showFloatingAi === true;
```

```ts
/** 根据订单状态返回展示文本 */
const getOrderStatusText = (status: OrderStatus): string => {
  if (!status) {
    return '未知状态';
  }

  return status === 'paid' ? '已支付' : '待支付';
};
```

## 交付前检查

- 检查是否出现 `function`、`var`、隐式返回箭头函数或无注释函数。
- 检查函数命名、文件命名、早返回及 `const` 使用是否符合规范。
- 检查复杂对象、数据推导和状态声明是否附有中文注释。
