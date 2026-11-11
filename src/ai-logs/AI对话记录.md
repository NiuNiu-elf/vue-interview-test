# AI 辅助开发对话记录 (原样归档)

**项目名称**：Windows 风格计算器
**技术栈**：Vue 3 + Vite
**核心考核点**：AI 调度能力、需求拆分能力、Git 颗粒度控制

> 说明：本记录原样归档了在开发过程中，我作为开发者与 AI 之间的核心业务逻辑对话。环境配置及基础语法类问题未纳入此归档，仅保留核心架构设计与逻辑推演过程。

---

## 阶段一：架构规划与 AI 调度策略

**[开发者]：**
我需要基于 Vue 3 开发一个 Windows 风格的计算器，并使用 Git 进行版本管理。HR 的核心要求是“考核 AI 调度和 Git 颗粒度”。请你帮我将整个开发流程拆分为多个极小的、可独立提交的阶段，确保每次 commit 只做一件事。

**[AI]：**
为了最大化满足“小颗粒度”的工程规范要求，建议将项目拆分为三大阶段，共 12 个微步骤：
1. **布局阶段**：初始化 -> 静态骨架(HTML) -> 网格样式(CSS)。
2. **逻辑阶段**：数字输入 -> 清除功能 -> 暂存状态 -> 等号运算 -> 连续运算 -> 边界处理。
3. **样式与优化阶段**：按键跨列对齐 -> 交互反馈 -> 键盘支持 -> 文档完善。
您可以按此顺序，每次让我只输出一个步骤的代码。

**[开发者]：**
ok，接下来请先给出第 1 步和第 2 步：初始化项目，并写出完全静态的计算器 HTML 结构和基础的 CSS Grid 布局。

*(...此处省略基础 HTML 和 CSS 的生成过程...)*

---

## 阶段二：核心状态机与逻辑推演

**[开发者]：**
继续我们现在接着下一步搞逻辑阶段。我需要在 Vue 的 `<script setup>` 中设计一个状态机。
请帮我实现第 5 步：**暂存第一个操作数和运算符**。
要求：定义 `currentValue`、`firstOperand`、`operator` 以及一个 `isWaitingForSecond` 标志位。按下运算符时，将当前屏幕的值存入 `firstOperand`，并让屏幕准备接收第二操作数。

**[AI]：**
为您设计了如下响应式状态与处理函数：
```javascript
const currentValue = ref('0')
const firstOperand = ref(null)
const operator = ref(null)
const isWaitingForSecond = ref(false)

const setOperator = (op) => {
  firstOperand.value = parseFloat(currentValue.value)
  operator.value = op
  isWaitingForSecond.value = true
}
当用户再次点击数字时，isWaitingForSecond 会拦截输入，用新数字替换屏幕内容。

[开发者]：
继续到第 7 步：支持连续运算。
如果在输入第二个数之前，用户又按了一次运算符（例如输入 1+2+），系统应该先结算 1+2，再用结果继续运算。请修改 setOperator 函数。

[AI]：
为了实现连续运算，需要在切换运算符之前，检查是否已经存在历史运算，并调用计算函数：

javascript
const setOperator = (op) => {
  // 核心修改：如果之前已有运算符且已输入了第二数，先结算上一步
  if (operator.value !== null && !isWaitingForSecond.value) {
    calculate() 
  }
  firstOperand.value = parseFloat(currentValue.value)
  operator.value = op
  isWaitingForSecond.value = true
}
[开发者]：
继续到第 8 步：处理除以 0 的情况。
在 calculate 函数的 / 分支中，如果除数为 0，请拦截计算并抛出中文提示“不能除以零”，同时重置状态防止后续报错。

(...此处省略 AI 给出的 try-catch 或 if-else 边界拦截代码...)

阶段三：交互优化与样式精修
[开发者]：
进入样式阶段。现在样式比较丑，目前网格是 4 列，请你帮我按照标准 Windows 计算器，在第 11 步帮我修改 HTML 和 CSS。

[AI]：
在 HTML 中为 0 的按钮添加 class="zero"，并在 CSS 中使用 Grid 的 span 属性：

css
.zero {
  grid-column: span 2;
}
这样既保持了整体布局，又完美还原了 UI 细节。

[开发者]：
最后一步交互优化：请在第 10 步绑定键盘事件。我需要监听数字键、运算符、Enter(等号) 以及 Escape/C(清空)。要求使用 onMounted 和 onUnmounted 确保生命周期安全。

[AI]：
为您添加了键盘事件监听器 handleKeydown，并挂载在 window 对象上：

javascript
const handleKeydown = (event) => {
  const key = event.key;
  if (/^[0-9.]$/.test(key)) appendNumber(key)
  else if (['+', '-', '*', '/'].includes(key)) setOperator(key)
  else if (key === 'Enter' || key === '=') calculate()
  else if (key.toLowerCase() === 'c' || key === 'Escape') clearDisplay()
}
onMounted(() => window.addEventListener('keydown', handleKeydown))
onUnmounted(() => window.removeEventListener('keydown', handleKeydown))