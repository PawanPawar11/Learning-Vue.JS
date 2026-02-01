<script setup>
import { ref } from "vue";

const steps = ref(["Step 1", "Step 2", "Step 3"]);
const currentStep = ref(0);

const prevStep = () => {
  if (currentStep.value > 0) {
    currentStep.value--;
  }
};

const nextStep = () => {
  if (currentStep.value < steps.value.length - 1) {
    currentStep.value++;
  }
};
</script>

<template>
  <div class="progress-container">
    <div class="progress-bar">
      <div
        v-for="(step, index) in steps"
        :key="index"
        :class="{ 'step-active': index === currentStep }"
      >
        {{ step }}
      </div>
    </div>

    <div class="controls">
      <button @click="prevStep" :disabled="currentStep === 0" class="btn">Previous</button>
      <button @click="nextStep" :disabled="currentStep === steps.length - 1" class="btn">
        Next
      </button>
    </div>
  </div>
</template>

<style scoped>
.progress-container {
  max-width: 400px;
  margin: 50px auto;
}

.progress-bar {
  display: flex;
  background-color: #e8eef3;
  border-radius: 18px;
}

.progress-bar div {
  flex: 1;
  text-align: center;
  padding: 15px;
  color: #64748b;
  font-weight: 500;
}

.progress-bar div:not(:last-child) {
  border-right: 2px solid #fff;
}

.step-active {
  background-color: #3b82f6;
  color: #fff !important;
}

.controls {
  margin-top: 20px;
  text-align: center;
}

.btn {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #3b82f6;
  color: #fff;
  border: none;
  border-radius: 5px;
  margin: 0 5px;
}

.btn:disabled {
  background-color: #cbd5e1;
  cursor: not-allowed;
}
</style>
