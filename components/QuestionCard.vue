<template>
  <div class="question-card">
    <h2 class="word">{{ question.word }}</h2>
    <p class="example" v-if="question.example">{{ question.example }}</p>
    <div class="options">
      <button
        v-for="(option, index) in question.options"
        :key="index"
        class="option-btn"
        :class="{
          correct: selectedOption === option && option === question.answer,
          wrong: selectedOption === option && option !== question.answer,
        }"
        :disabled="selectedOption !== null"
        @click="selectOption(option)"
      >
        {{ option }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref, computed } from "vue";

const props = defineProps({
  question: {
    type: Object,
    required: true,
  },
  selectedOption: {
    type: String,
    default: null,
  },
});

const emit = defineEmits(["select"]);

const selectOption = (option) => {
  emit("select", option);
};
</script>

<style scoped>
.question-card {
  background-color: #e8f5e8;
  border-radius: 12px;
  padding: 24px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  width: 100%;
  text-align: center;
}

.word {
  font-size: 32px;
  font-weight: bold;
  margin-bottom: 12px;
  color: #333;
}

.example {
  font-size: 14px;
  color: #666;
  margin-bottom: 24px;
  font-style: italic;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 8px;
  align-items: center;
}

.option-btn {
  padding: 12px 0;
  border: none;
  border-radius: 8px;
  background-color: white;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s ease;
  width: 100%;
  max-width: 300px;
  text-align: center;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.option-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
}

.option-btn:disabled {
  cursor: not-allowed;
}

.option-btn.correct {
  background-color: #c8e6c9;
  color: #2e7d32;
}

.option-btn.wrong {
  background-color: #ffcdd2;
  color: #c62828;
}

@media (max-width: 768px) {
  .question-card {
    padding: 16px;
  }

  .word {
    font-size: 28px;
  }

  .example {
    font-size: 12px;
  }

  .option-btn {
    padding: 10px 0;
    font-size: 14px;
    max-width: 250px;
  }
}
</style>