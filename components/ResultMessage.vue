<template>
  <div class="result-message" :class="{ 'show': show }">
    <div v-if="!isComplete" class="result">
      <p class="result-text" :class="resultType">{{ resultText }}</p>
      <p class="score">当前得分: {{ score }}/{{ total }}</p>
    </div>
    <div v-else class="complete">
      <h3>练习完成!</h3>
      <p class="final-score">总分: {{ score }}/{{ total }}</p>
      <p class="encouragement">{{ encouragement }}</p>
    </div>
  </div>
</template>

<script setup>
import { defineProps, computed } from 'vue';

const props = defineProps({
  show: {
    type: Boolean,
    default: false
  },
  resultType: {
    type: String,
    default: ''
  },
  resultText: {
    type: String,
    default: ''
  },
  score: {
    type: Number,
    default: 0
  },
  total: {
    type: Number,
    default: 0
  },
  isComplete: {
    type: Boolean,
    default: false
  }
});

const encouragement = computed(() => {
  if (props.score === props.total) {
    return '太棒了！你全部答对了！';
  } else if (props.score >= props.total * 0.7) {
    return '做得很好！继续加油！';
  } else {
    return '别灰心，多练习一定会进步的！';
  }
});
</script>

<style scoped>
.result-message {
  background-color: white;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  width: 100%;
  opacity: 0;
  transform: translateY(-10px);
  transition: all 0.3s ease;
}

.result-message.show {
  opacity: 1;
  transform: translateY(0);
}

.result-text {
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 8px;
}

.result-text.correct {
  color: #28a745;
}

.result-text.wrong {
  color: #dc3545;
}

.score {
  text-align: center;
  color: #666;
  margin: 0;
}

.complete {
  text-align: center;
}

.complete h3 {
  color: #333;
  margin-bottom: 12px;
}

.final-score {
  font-size: 20px;
  font-weight: bold;
  color: #007bff;
  margin-bottom: 12px;
}

.encouragement {
  color: #666;
  font-size: 16px;
  margin: 0;
}

@media (max-width: 768px) {
  .result-message {
    padding: 12px;
  }
  
  .result-text {
    font-size: 16px;
  }
  
  .final-score {
    font-size: 18px;
  }
}
</style>