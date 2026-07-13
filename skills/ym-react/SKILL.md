---
name: ym-react
description: 编写、修改或审查 React 与 TSX 组件时应用组件拆分、状态管理、事件处理和函数实现规范。
---

# React 规范

- 按职责拆分组件、类型、hooks、常量和工具函数，避免将全部内容放入一个 `.tsx` 文件。
- 保持组件短小、单一职责、名称清晰；将复杂界面拆分成可独立理解和测试的子组件。
- 为 React 组件添加简短中文注释说明职责。
- 为事件处理函数使用 `handle` 前缀。
- 为组件和辅助函数使用箭头函数、代码块和显式 `return`。
- 仅在需要可变状态或副作用时使用 React Hook；优先通过纯函数计算派生数据。
- 在相邻 JSX 同级元素之间保留一行空行。

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

```tsx
<div>
  <span>123</span>

  <span>456</span>
</div>
```

## 交付前检查

- 检查 React 文件是否职责过多；必要时拆分为多个语义清晰的文件。
- 检查组件、事件处理函数和辅助函数是否符合既定命名与实现规则。
- 检查相邻 JSX 同级元素之间保留一行空行。
