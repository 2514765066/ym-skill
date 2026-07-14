---
name: ym-rules
description: 编写、修改或审查 TypeScript、JavaScript、React、TSX、HTML、Vue 模板与 Tailwind CSS v4 时，统一应用个人代码、组件、Tailwind 类名和中文技术回复规范。
---

# YM 开发规则

## 回复

- 使用中文回复。
- 不写问候语、客套话、无意义语气词或重复总结。
- 直接给出结果、变更说明、风险或待确认事项。

## 通用代码

- 优先使用 TypeScript；无法使用时使用现代 JavaScript。
- 优先函数式编程：保持函数纯粹、数据不可变，使用 `map`、`filter`、`reduce` 等方式转换数据。
- 变量和函数使用小驼峰命名；文件和目录使用短横线命名；组件使用帕斯卡命名法，组件文件使用短横线命名。
- 使用表达业务语义的函数名；事件处理函数以 `handle` 开头，例如 `handleSubmitOrder`。
- 禁止使用 `function` 与 `var`；普通函数和回调均使用带代码块和显式 `return` 的箭头函数。
- 需要定义类时使用 `class`，不要用函数模拟类。
- 为所有函数添加简短中文注释说明职责。
- 优先使用 `const`，避免使用 `let`。
- 将错误处理、无效输入和退出条件放在函数前部，使用早返回减少嵌套。

### 复杂声明注释

- 为复杂对象、数据推导和状态声明添加简短中文注释，说明其业务含义或判定目的。
- 多行表达式、深层属性访问、可选链、空值回退或包含多个逻辑条件的声明必须添加中文注释。
- 简单字面量和语义明确的直接赋值无需添加注释。

```ts
/** 优先使用显式宽度，缺省时回退到文本节点默认宽度 */
const editorWidth =
  explicitNodeSize.width ?? textNodeConfig.defaultSize.width;

/** 根据订单状态返回展示文本 */
const getOrderStatusText = (status: OrderStatus): string => {
  if (!status) {
    return '未知状态';
  }

  return status === 'paid' ? '已支付' : '待支付';
};
```

## React 与 TSX

- 按职责拆分组件、类型、hooks、常量和工具函数，避免将全部内容放入一个 `.tsx` 文件。
- 保持组件短小、单一职责、名称清晰；将复杂界面拆分成可独立理解和测试的子组件。
- 为 React 组件添加简短中文注释说明职责。
- 仅在需要可变状态或副作用时使用 React Hook；优先通过纯函数计算派生数据。
- 在相邻 JSX 同级元素之间保留一行空行。

```tsx
/** 展示订单金额摘要 */
export const OrderSummary = ({ total }: OrderSummaryProps): JSX.Element => {
  if (total < 0) {
    return <span>金额无效</span>;
  }

  return <span>订单总额：{total}</span>;
};
```

## Tailwind CSS v4

- 按以下分组从前到后排列 `class` 或 `className`：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
- 将组件样式类、业务状态类和第三方组件覆写类等自定义类名置于最前。
- 尺寸类包括 `w-*`、`h-*`、`size-*`；定位类包括 `relative`、`absolute`、`fixed`、`sticky`、`top-*`、`right-*`、`bottom-*`、`left-*`、`inset-*` 与 `z-*`。
- 布局与对齐类包括 `flex`、`grid`、`block`、`items-center`、`justify-center` 与 `gap-*`。
- 将 `bg-*`、`text-*`、`border-*`、`ring-*`、`fill-*`、`stroke-*`、深色模式和其他主题变体置于最后。
- 同一分组内按语义和阅读顺序保持稳定；不要为了机械排序打散表达同一意图的类名。

```tsx
<div className="order-card w-full h-full relative top-0 left-0 flex items-center justify-center bg-white text-slate-900 dark:bg-slate-900 dark:text-white" />
```

## 交付前检查

- 检查是否出现 `function`、`var`、隐式返回箭头函数或缺少职责注释的函数。
- 检查命名、早返回、`const`、复杂声明注释和 React 组件拆分是否符合规范。
- 检查相邻 JSX 同级元素之间保留一行空行。
- 检查 Tailwind CSS v4 类名顺序是否为：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
