<script setup>
import { ref } from "vue";

const quotes = ref([
  { text: "Talk is cheap. Show me the code.", author: "Linus Torvalds" },
  { text: "Programs must be written for people to read.", author: "Harold Abelson" },
  { text: "First, solve the problem. Then, write the code.", author: "John Johnson" },
  { text: "Experience is the name everyone gives to their mistakes.", author: "Oscar Wilde" },
  { text: "Code is like humor. When you have to explain it, it’s bad.", author: "Cory House" },
]);

const currentQuote = ref(quotes.value[0]);

const generateRandomQuote = () => {
  const randomIndex = Math.floor(Math.random() * quotes.value.length);
  currentQuote.value = quotes.value[randomIndex];
};
</script>

<template>
  <div class="container">
    <h1>💬 Random Quote Generator</h1>

    <div class="quote-box">
      <p class="quote">"{{ currentQuote.text }}"</p>
      <p class="author">- {{ currentQuote.author }}</p>
    </div>

    <button @click="generateRandomQuote">New Quote</button>
  </div>
</template>

<style scoped>
.container {
  max-width: 500px;
  margin: 80px auto;
  text-align: center;
  font-family: sans-serif;
}

.quote-box {
  background: #f5f5f5;
  padding: 20px;
  border-radius: 10px;
  margin: 20px 0;
}

.quote {
  font-size: 18px;
  font-style: italic;
}

.author {
  margin-top: 10px;
  font-weight: bold;
}

button {
  padding: 10px 20px;
  border: none;
  background: #42b883;
  color: white;
  font-size: 16px;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background: #369f6b;
}
</style>
