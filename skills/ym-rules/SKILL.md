---
name: ym-rules
description: 编写、修改或审查 TypeScript、JavaScript、React、TSX、Vue 3 单文件组件、HTML 与 Tailwind CSS v4 时，统一应用个人代码、组件、Vue Composition API、Tailwind 类名和中文技术回复规范。
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
- 不要在函数调用前使用 `void`；直接调用函数，或在确实需要时处理其返回值。
- 函数返回类型能够由 TypeScript 推断时不要显式标注；仅在公开契约、重载或推断不准确时添加返回类型。
- 需要定义类时使用 `class`，不要用函数模拟类。
- 为所有函数添加简短中文单行注释说明职责，使用 `//`，不要使用 `/** */`。
- 为所有数据声明添加简短中文单行注释，说明数据用途或业务含义，使用 `//`，不要使用 `/** */`。
- 优先使用 `const`，避免使用 `let`。
- 将错误处理、无效输入和退出条件放在函数前部，使用早返回减少嵌套。

### 数据声明注释

- 为所有常量、变量、对象、数组、配置、状态和派生值添加简短中文注释，说明数据用途、业务含义或判定目的。
- 即使是简单字面量或语义明确的直接赋值，也必须添加中文注释。
- 多行表达式、深层属性访问、可选链、空值回退或包含多个逻辑条件的声明，注释应说明数据来源或计算规则。

```ts
// 订单详情接口地址
const orderDetailEndpoint = '/api/orders/detail';

// 优先使用显式宽度，缺省时回退到文本节点默认宽度
const editorWidth =
  explicitNodeSize.width ?? textNodeConfig.defaultSize.width;

// 根据订单状态返回展示文本
const getOrderStatusText = (status: OrderStatus) => {
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
// 展示订单金额摘要
export const OrderSummary = ({ total }: OrderSummaryProps) => {
  if (total < 0) {
    return <span>金额无效</span>;
  }

  return <span>订单总额：{total}</span>;
};
```

## Vue 3

- 创建 `.vue` 文件时，必须使用以下单文件组件模板；保持区块顺序、`script setup`、`scoped` 与 `lang='scss'` 不变。

```vue
<template>

</template>

<script setup lang='ts'>

</script>

<style scoped lang='scss'>

</style>
```

- 状态只能使用 `ref`；禁止使用 `reactive`。对象和数组状态同样使用 `ref`，通过 `.value` 读取或更新。
- 简单组件直接创建为 `组件名.vue`，例如 `OrderSummary.vue`；复杂组件创建为 `组件名/index.vue`，并在同级目录继续拆分子组件、类型、常量和工具函数。
- 优先使用 Vue 3 编译器宏定义组件契约，例如 `defineProps`、`defineEmits`、`defineModel`、`defineSlots` 与 `defineExpose`；为宏定义使用内联对象泛型，例如 `defineProps<{ title: string }>()`，不要传入命名的 Props 或 Emits 类型。
- `ref` 和 `computed` 能从初始值或返回值推断类型时不要添加泛型；仅在初始值无法表达完整类型时添加泛型，例如 `ref<Order | null>(null)`。避免以断言或 `any` 绕过类型检查。

```vue
<script setup lang='ts'>
// 订单组件入参
const props = defineProps<{
  total: number;
}>();

// 订单操作事件
const emit = defineEmits<{
  select: [order: Order];
}>();

// 当前选中的订单
const selectedOrder = ref<Order | null>(null);

// 是否已选择订单
const hasSelectedOrder = computed(() => {
  return selectedOrder.value !== null;
});

// 处理订单选择
const handleSelectOrder = (order: Order) => {
  selectedOrder.value = order;
  emit('select', order);
};
</script>
```

## 元素结构与样式

- 使用最少且语义正确的元素。优先使用 `section`、`main`、`header`、`footer` 等语义化标签；页面或独立区块需要完整结构时，按 `section > header > main > footer` 的顺序组织。
- 不要为无布局、无语义、无样式需求的内容增加容器；能直接渲染内容时不要额外嵌套 `section` 或 `div`。
- 优先通过 `flex`、`grid` 和间距工具类完成布局，以减少嵌套层级；例如双列内容需要左右分布时，使用 `flex` 配合右侧元素的 `ml-auto`，不要为此增加额外容器或使用无必要的 `justify-between`。
- 样式保持最少且服务于当前布局和交互；不要添加无效、重复或仅为装饰而没有设计依据的样式。

```vue
<section>
  <header></header>

  <main></main>

  <footer></footer>
</section>

<section class="flex">
  <div></div>

  <div class="ml-auto"></div>
</section>
```

## Tailwind CSS v4

- 按以下分组从前到后排列 `class` 或 `className`：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
- 将组件样式类、业务状态类和第三方组件覆写类等自定义类名置于最前。
- 尺寸类包括 `w-*`、`h-*`、`size-*`；定位类包括 `relative`、`absolute`、`fixed`、`sticky`、`top-*`、`right-*`、`bottom-*`、`left-*`、`inset-*` 与 `z-*`。
- 布局与对齐类包括 `flex`、`grid`、`block`、`items-center`、`justify-center` 与 `gap-*`。
- 将 `bg-*`、`text-*`、`border-*`、`ring-*`、`fill-*`、`stroke-*`、深色模式和其他主题变体置于最后。
- 同一分组内按语义和阅读顺序保持稳定；不要为了机械排序打散表达同一意图的类名。
- 合并等价的间距和尺寸类，例如使用 `p-4` 代替 `px-4 py-4`，使用 `size-full` 代替 `w-full h-full`。
- 项目使用 Tailwind 时，颜色优先复用 Tailwind CSS v4 主题变量或内置调色板，保持全局统一；不要在元素上随意使用 `text-white`、`text-[#262626]` 等孤立颜色值。

```tsx
<div className="order-card size-full relative top-0 left-0 flex items-center justify-center bg-slate-100 text-slate-900" />
```

## 交付前检查

- 检查是否出现 `function`、`var`、函数调用前的 `void`、隐式返回箭头函数或缺少职责注释的函数。
- 检查能够推断的函数返回类型、`ref` 和 `computed` 是否省略了多余类型标注。
- 检查所有数据声明是否有说明用途或业务含义的简短中文注释。
- 检查命名、早返回、`const`、数据声明注释和 React 组件拆分是否符合规范。
- 检查相邻 JSX 同级元素之间保留一行空行。
- 检查 `.vue` 文件是否使用规定的单文件组件模板、`ref` 状态，以及内联对象泛型的 Vue 3 宏定义。
- 检查简单 Vue 组件是否使用 `组件名.vue`，复杂 Vue 组件是否使用 `组件名/index.vue` 并合理拆分子组件。
- 检查元素结构是否使用最少的语义化标签，并通过 `flex` 或 `grid` 避免无用嵌套。
- 检查 Tailwind 类是否合并了等价值，颜色是否复用了项目主题变量或内置调色板。
- 检查 Tailwind CSS v4 类名顺序是否为：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
