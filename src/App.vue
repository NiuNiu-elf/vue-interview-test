<template>
  <div class="calculator">
    <div class="display">{{ currentValue }}</div>
    <div class="buttons">
      <button @click="appendNumber('7')">7</button><button @click="appendNumber('8')">8</button><button @click="appendNumber('9')">9</button><button class="operator" @click="setOperator('/')">÷</button>
      <button @click="appendNumber('4')">4</button><button @click="appendNumber('5')">5</button><button @click="appendNumber('6')">6</button><button class="operator" @click="setOperator('*')">×</button>
      <button @click="appendNumber('1')">1</button><button @click="appendNumber('2')">2</button><button @click="appendNumber('3')">3</button><button class="operator" @click="setOperator('-')">-</button>
      <button @click="appendNumber('0')">0</button><button @click="appendNumber('.')">.</button><button class="operator" @click="setOperator('+')">+</button>
      <button class="clear" @click="clearDisplay">C</button><button class="operator" @click="calculate">=</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const currentValue = ref('0')
const firstOperand = ref(null)
const operator = ref(null)
const isWaitingForSecond = ref(false)

const appendNumber = (num) => {
  if (isWaitingForSecond.value) {
    currentValue.value = num === '.' ? '0.' : num
    isWaitingForSecond.value = false
  } else {
    if (currentValue.value === '0' && num !== '.') currentValue.value = num
    else currentValue.value += num
  }
}
const clearDisplay = () => {
  currentValue.value = '0'
  firstOperand.value = null
  operator.value = null
  isWaitingForSecond.value = false
}
const setOperator = (op) => {
  if (operator.value !== null && !isWaitingForSecond.value) calculate()
  firstOperand.value = parseFloat(currentValue.value)
  operator.value = op
  isWaitingForSecond.value = true
}
const calculate = () => {
  if (operator.value === null || firstOperand.value === null) return
  const secondOperand = parseFloat(currentValue.value)
  let result = 0
  switch (operator.value) {
    case '+': result = firstOperand.value + secondOperand; break
    case '-': result = firstOperand.value - secondOperand; break
    case '*': result = firstOperand.value * secondOperand; break
    case '/': 
      if (secondOperand === 0) {
        currentValue.value = "不能除以零"
        firstOperand.value = null
        operator.value = null
        isWaitingForSecond.value = false
        return
      }
      result = firstOperand.value / secondOperand; break
  }
  currentValue.value = String(result)
  firstOperand.value = null
  operator.value = null
  isWaitingForSecond.value = false
}
</script>

<style scoped>
.calculator { width: 320px; margin: 50px auto; background-color: #202020; border-radius: 8px; padding: 20px; font-family: 'Segoe UI', sans-serif; }
.display { background-color: #202020; color: white; font-size: 48px; text-align: right; padding: 10px; margin-bottom: 20px; min-height: 70px; }
.buttons { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
.buttons button { font-size: 20px; padding: 15px; border: none; border-radius: 6px; background-color: #3b3b3b; color: white; cursor: pointer; }
.buttons button:hover { background-color: #4a4a4a; }
.operator { background-color: #323232 !important; }
.operator:hover { background-color: #404040 !important; }
.clear { background-color: #d32f2f !important; }
.clear:hover { background-color: #e53935 !important; }
</style>