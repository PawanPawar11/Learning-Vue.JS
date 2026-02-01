<script setup>
import axios from "axios";
import { ref } from "vue";

const joke = ref("");
const isLoading = ref(false);

const fetchJokes = async () => {
  isLoading.value = true;

  try {
    const response = await axios.get("https://icanhazdadjoke.com/", {
      headers: { Accept: "application/json" },
    });
    joke.value = response.data.joke;
  } catch (error) {
    console.log("Error occurred while fetching dad joke!", error);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <div class="dad-joke-container">
    <h1 class="dad-joke-title">Dad Jokes</h1>
    <button @click="fetchJokes" class="get-joke-button" :disabled="isLoading">
      {{ isLoading ? "Loading..." : "Get Dad Joke" }}
    </button>
    <div v-if="joke" class="dad-joke">
      {{ joke }}
    </div>
  </div>
</template>

<style scoped>
.dad-joke-container {
  max-width: 600px;
  margin: 50px auto;
}

.dad-joke-container {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

.get-joke-button {
  padding: 10px 15px;
  font-size: 16px;
  background-color: #4caf50;
  transition: background-color 0.3s ease-in-out;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.get-joke-button:not(:disabled):hover {
  background-color: #45a049;
}

.get-joke-button:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}

.dad-joke {
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 15px;
  margin-top: 20px;
  background-color: #f9f9f9;
  color: #333;
}
</style>
