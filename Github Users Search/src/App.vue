<script setup>
import { ref } from "vue";

const username = ref("");
const userProfile = ref(null);
const error = ref(null);

const getUserProfile = async () => {
  try {
    const response = await fetch(`https://api.github.com/users/${username.value}`);

    const data = await response.json();

    if (response.ok) {
      userProfile.value = data;
      error.value = null;
    } else {
      userProfile.value = null;
      error.value = `Error: ${data.message}`;
    }
  } catch (error) {
    console.log("Error while fetching user info", error);
    error.value = "An error occurred while fetching data!";
  }
};
</script>

<template>
  <div class="github-profile-viewer">
    <h1 class="app-title">Github User Profile Viewer</h1>

    <div class="input-container">
      <input
        type="text"
        v-model="username"
        placeholder="Enter your github username"
        @input="getUserProfile"
      />
    </div>

    <div v-if="userProfile" class="user-profile">
      <img :src="userProfile.avatar_url" :alt="userProfile.login" />

      <div class="user-details">
        <p>
          <strong>Name: {{ userProfile.name }}</strong>
        </p>
        <p>
          <strong>Location: {{ userProfile.location }}</strong>
        </p>
        <p>
          <strong>Following: {{ userProfile.following }}</strong>
        </p>
        <p>
          <strong>Followers: {{ userProfile.followers }}</strong>
        </p>
        <p>
          <strong>Public Repos: {{ userProfile.public_repos }}</strong>
        </p>
      </div>

      <div v-if="error" class="error-message">
        <p>{{ error }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Inter", sans-serif;
}

.github-profile-viewer {
  max-width: 600px;
  margin: 50px auto;
  padding: 40px;
  text-align: center;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.app-title {
  font-size: 28px;
  margin-bottom: 30px;
  color: #1a202c;
  font-weight: 700;
}

.input-container {
  margin-bottom: 30px;
}

input {
  width: 100%;
  padding: 14px 18px;
  font-size: 16px;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  outline: none;
  transition: all 0.3s ease;
}

input:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.user-profile {
  margin-top: 30px;
}

img {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  margin-bottom: 25px;
  border: 4px solid #f1f5f9;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.user-details {
  text-align: left;
  background: #f8fafc;
  padding: 24px;
  border-radius: 12px;
}

p {
  font-size: 16px;
  margin-bottom: 12px;
  color: #475569;
}

p:last-child {
  margin-bottom: 0;
}

strong {
  color: #1e293b;
  font-weight: 600;
}

.error-message {
  color: #ef4444;
  margin-top: 20px;
  background: #fee2e2;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
}
</style>
