---
name: ym-tailwind-class-order
description: 编写、修改或审查使用 Tailwind CSS v4 的 HTML、JSX、TSX 与 Vue 模板时，统一 class 和 className 中工具类的排列顺序。
---

# Tailwind CSS v4 类名顺序

- 按以下分组从前到后排列 `class` 或 `className`：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
- 将组件样式类、业务状态类和第三方组件覆写类等自定义类名置于最前。
- 将 `w-*`、`h-*`、`size-*` 等尺寸类置于自定义类名之后，例如 `w-full`、`h-full`、`size-full`。
- 将 `relative`、`absolute`、`fixed`、`sticky`、`top-*`、`right-*`、`bottom-*`、`left-*`、`inset-*`、`z-*` 等定位类置于尺寸之后。
- 将 `flex`、`grid`、`block`、`items-center`、`justify-center`、`gap-*` 等布局与对齐类置于定位之后。
- 将 `bg-*`、`text-*`、`border-*`、`ring-*`、`fill-*`、`stroke-*`、深色模式和其他主题变体置于最后。
- 在同一分组内按语义和阅读顺序保持稳定；不要为了机械排序打散表达同一意图的类名。

```tsx
// 自定义类名 → 尺寸 → 定位 → 布局与对齐 → 颜色与主题
<div className="order-card w-full h-full relative top-0 left-0 flex items-center justify-center bg-white text-slate-900 dark:bg-slate-900 dark:text-white" />
```

## 交付前检查

- 检查 Tailwind CSS v4 类名是否遵循：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
