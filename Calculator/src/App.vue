<script setup>
import { ref } from "vue";

// calculator display
const display = ref("");

// append value (numbers & operators)
const appendValue = (value) => {
  display.value += value;
};

// clear display
const clearDisplay = () => {
  display.value = "";
};

// calculate result
const calculate = () => {
  try {
    display.value = eval(display.value);
  } catch {
    display.value = "Error";
  }
};
</script>

<template>
  <div class="calculator">
    <!-- Display -->
    <input class="display" type="text" v-model="display" disabled />

    <!-- Buttons -->
    <div class="buttons">
      <button class="clear-btn" @click="clearDisplay">C</button>

      <button @click="appendValue('7')">7</button>
      <button @click="appendValue('8')">8</button>
      <button @click="appendValue('9')">9</button>
      <button @click="appendValue('/')">÷</button>

      <button @click="appendValue('4')">4</button>
      <button @click="appendValue('5')">5</button>
      <button @click="appendValue('6')">6</button>
      <button @click="appendValue('*')">×</button>

      <button @click="appendValue('1')">1</button>
      <button @click="appendValue('2')">2</button>
      <button @click="appendValue('3')">3</button>
      <button @click="appendValue('-')">−</button>

      <button @click="appendValue('0')">0</button>
      <button @click="appendValue('.')">.</button>
      <button @click="calculate">=</button>
      <button @click="appendValue('+')">+</button>
    </div>
  </div>
</template>

<style>
.calculator {
  width: 260px;
  margin: 80px auto;
  padding: 15px;
  border-radius: 10px;
  background: #f4f4f4;
  font-family: sans-serif;
}

.display {
  width: 100%;
  height: 50px;
  margin-bottom: 10px;
  font-size: 20px;
  text-align: right;
  padding: 5px;
  box-sizing: border-box;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}

button {
  padding: 15px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background: #fff;
}

button:hover {
  background: #ddd;
}

.clear-btn {
  grid-column: span 4;
  background: #ff6b6b;
  color: white;
  font-weight: bold;
}

.clear-btn:hover {
  background: #ff5252;
}
</style>
