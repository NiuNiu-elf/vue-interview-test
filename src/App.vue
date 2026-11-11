<template>
  <div class="calculator">
    <div class="display">0</div>
    <div class="buttons">
      <button>7</button><button>8</button><button>9</button><button class="operator">÷</button>
      <button>4</button><button>5</button><button>6</button><button class="operator">×</button>
      <button>1</button><button>2</button><button>3</button><button class="operator">-</button>
      <button>0</button><button>.</button><button class="operator">+</button>
      <button class="clear">C</button><button class="operator">=</button>
    </div>
  </div>
</template>

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