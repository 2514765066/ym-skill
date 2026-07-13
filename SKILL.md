---
name: ym-skill
description: 个人编码习惯。以中文回复并编写 TypeScript、JavaScript 或 React 代码时，执行函数式编程、函数、命名和组件拆分规范。
---

# 个人编码习惯

## 回复要求

- 全部使用中文回复。
- 不写问候语、客套话、无意义语气词或重复总结。
- 直接给出结果、变更说明、风险或待确认事项。

## 通用代码规范

- 优先使用 TypeScript；无法使用时使用现代 JavaScript。
- 优先函数式编程：保持函数纯粹、数据不可变，使用 `map`、`filter`、`reduce` 等方式转换数据。

## 命名规范

- 变量和函数使用小驼峰命名；文件和目录使用短横线命名。
- 函数名表达业务语义；事件处理函数以 `handle` 开头，例如 `handleSubmitOrder`。
- 组件名使用帕斯卡命名法，组件文件使用短横线命名。

## 函数要求

- 禁止使用 `function` 关键字；所有普通函数和回调使用箭头函数。
- 箭头函数必须使用代码块，并显式写出 `return`。
- 需要定义类时使用 `class`，不要用函数模拟类。
- 所有函数添加简短中文注释说明职责；React 组件也添加简短中文注释。
- 能用 `const` 时不用 `let`；禁止使用 `var`。
- 将错误处理、无效输入和退出条件放在函数前部，使用早返回减少嵌套。

## 函数模板

```ts
/** 根据订单状态返回展示文本 */
const getOrderStatusText = (status: OrderStatus): string => {
  if (!status) {
    return '未知状态';
  }

  return status === 'paid' ? '已支付' : '待支付';
};

/** 处理订单提交事件 */
const handleSubmitOrder = (order: Order): Result => {
  if (!order.id) {
    return { success: false, message: '订单编号不能为空' };
  }

  return { success: true, message: '提交成功' };
};
```

## React 规范

- 按职责拆分组件、类型、hooks、常量和工具函数，避免将全部内容放入一个 `.tsx` 文件。
- 组件保持短小、单一职责、名称清晰；复杂界面拆分成可独立理解和测试的子组件。
- 事件处理函数使用 `handle` 前缀；组件和辅助函数均遵守箭头函数与显式 `return` 规则。
- 只在需要可变状态或副作用时使用 React Hook；派生数据优先通过纯函数计算。

```tsx
type OrderSummaryProps = {
  total: number;
};

/** 展示订单金额摘要 */
export const OrderSummary = ({ total }: OrderSummaryProps): JSX.Element => {
  if (total < 0) {
    return <span>金额无效</span>;
  }

  return <span>订单总额：{total}</span>;
};
```

## Tailwind CSS v4 类名顺序

- 编写 `class` 或 `className` 时，按以下分组从前到后排列：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
- 自定义类名必须置于最前，例如组件样式类、业务状态类或第三方组件覆写类。
- 尺寸类紧随其后，例如 `w-full`、`h-full`、`size-full` 及其他 `w-*`、`h-*`、`size-*`。
- 定位类置于尺寸之后，例如 `relative`、`absolute`、`fixed`、`sticky`、`top-*`、`right-*`、`bottom-*`、`left-*`、`inset-*`、`z-*`。
- 布局与对齐类置于定位之后，例如 `flex`、`grid`、`block`、`items-center`、`justify-center`、`gap-*`。
- 颜色和主题相关类必须放在最后，例如 `bg-*`、`text-*`、`border-*`、`ring-*`、`fill-*`、`stroke-*`、深色模式及其他主题变体。
- 同一分组内按语义和阅读顺序保持稳定；不要为了机械排序打散成对表达同一意图的类名。

```tsx
// 自定义类名 → 尺寸 → 定位 → 布局与对齐 → 颜色与主题
<div className="order-card w-full h-full relative top-0 left-0 flex items-center justify-center bg-white text-slate-900 dark:bg-slate-900 dark:text-white" />
```

## 交付前检查

- 检查是否出现 `function`、`var`、隐式返回箭头函数或无注释函数。
- 检查函数命名、文件命名、早返回及 `const` 使用是否符合规范。
- 检查 React 文件是否职责过多；必要时拆分为多个语义清晰的文件。
- 检查 Tailwind CSS v4 类名是否遵循：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
