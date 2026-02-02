<script setup>
import { ref } from "vue";

const searchQuery = ref("");
const searchResults = ref([]);
const isLoading = ref(false);
const error = ref(null);
const isDarkTheme = ref(false);

const searchWikipedia = async (query) => {
  const encodedQuery = encodeURIComponent(query);
  const endpoint = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${encodedQuery}`;

  try {
    isLoading.value = true;
    error.value = null; // Reset error on new search

    const response = await fetch(endpoint);
    const data = await response.json();

    if (data.query && data.query.search.length > 0) {
      searchResults.value = data.query.search;
    } else {
      searchResults.value = [];
      error.value = "No results found for your search.";
    }
  } catch (err) {
    console.error("Error fetching data:", err);
    searchResults.value = [];
    error.value = "Error occurred while fetching data!";
  } finally {
    isLoading.value = false;
  }
};

const submitSearch = () => {
  if (searchQuery.value.trim() !== "") {
    searchWikipedia(searchQuery.value);
  } else {
    searchResults.value = [];
    error.value = "Please enter a valid search term!";
  }
};

const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value;
};
</script>

<template>
  <div :class="['app-wrapper', { 'dark-theme': isDarkTheme }]">
    <div class="container">
      <header class="header-container">
        <h1>WikiSearch</h1>
        <button
          id="theme-toggler"
          @click="toggleTheme"
          :aria-label="isDarkTheme ? 'Switch to light mode' : 'Switch to dark mode'"
        >
          {{ isDarkTheme ? "☀️ Light" : "🌙 Dark" }}
        </button>
      </header>

      <form @submit.prevent="submitSearch" id="search-form">
        <input
          type="text"
          id="search-input"
          v-model="searchQuery"
          placeholder="What are you looking for?"
        />
        <button type="submit">Search</button>
      </form>

      <main class="search-results">
        <div v-if="isLoading" class="spinner">
          <div class="loader"></div>
          <p>Searching Wikipedia...</p>
        </div>

        <p v-if="error" class="error-message">{{ error }}</p>

        <div v-if="searchResults.length && !isLoading">
          <article v-for="result in searchResults" :key="result.pageid" class="result-item">
            <h3 class="result-title">
              <a
                :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
                target="_blank"
                rel="noopener"
              >
                {{ result.title }}
              </a>
            </h3>
            <a
              :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
              class="result-link"
              target="_blank"
              rel="noopener"
            >
              https://en.wikipedia.org/?curid={{ result.pageid }}
            </a>
            <p class="result-snippet" v-html="result.snippet + '...'"></p>
          </article>
        </div>
      </main>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap");

.app-wrapper {
  min-height: 100vh;
  width: 100%;
  background: #f8fafc;
  transition:
    background-color 0.4s ease,
    color 0.4s ease;
  font-family: "Inter", sans-serif;
  color: #1e293b;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 3rem 1.5rem;
}

.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2.5rem;
}

h1 {
  font-size: 2rem;
  font-weight: 800;
  background: linear-gradient(135deg, #3b82f6, #2563eb);
}

#search-form {
  display: flex;
  gap: 12px;
  margin-bottom: 3rem;
}

#search-input {
  flex: 1;
  padding: 16px 20px;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.2s;
}

#search-input:focus {
  border-color: #3b82f6;
}

button[type="submit"] {
  padding: 0 24px;
  background: #2563eb;
  color: white;
  border: none;
  border-radius: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.2s;
}

button[type="submit"]:hover {
  opacity: 0.9;
}

#theme-toggler {
  padding: 8px 16px;
  border-radius: 20px;
  border: 1px solid #e2e8f0;
  background: white;
  cursor: pointer;
  font-weight: 500;
}

.result-item {
  background: white;
  padding: 24px;
  border-radius: 16px;
  margin-bottom: 20px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.result-title a {
  color: #2563eb;
  text-decoration: none;
}

.result-link {
  font-size: 0.85rem;
  color: #10b981;
  text-decoration: none;
  display: block;
  margin: 4px 0 12px 0;
}

.result-snippet {
  font-size: 0.95rem;
  line-height: 1.6;
  color: #475569;
}

/* Specific styling for the Wikipedia highlight class */
:deep(.searchmatch) {
  font-weight: bold;
  background: rgba(59, 130, 246, 0.1);
  padding: 0 2px;
}

/* Dark Theme Overrides */
.dark-theme {
  background: #0f172a;
  color: #f1f5f9;
}

.dark-theme #theme-toggler {
  background: #1e293b;
  border-color: #334155;
  color: white;
}

.dark-theme .result-item {
  background: #1e293b;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.3);
}

.dark-theme .result-snippet {
  color: #94a3b8;
}

.dark-theme #search-input {
  background: #1e293b;
  border-color: #334155;
  color: white;
}

.error-message {
  padding: 1rem;
  background: #fee2e2;
  color: #991b1b;
  border-radius: 8px;
  text-align: center;
}

.dark-theme .error-message {
  background: #450a0a;
  color: #fecaca;
}

.spinner {
  text-align: center;
  padding: 3rem;
  color: #64748b;
}
</style>
