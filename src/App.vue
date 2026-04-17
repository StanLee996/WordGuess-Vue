<template>
  <div class="app">
    <div class="container">
      <h1 class="title">单词打卡</h1>

      <div class="category-buttons">
        <button
          v-for="category in categories"
          :key="category"
          class="category-btn"
          :class="{ active: currentCategory === category }"
          @click="selectCategory(category)"
        >
          {{ category }}
        </button>
      </div>

      <QuestionCard
        :question="currentQuestion"
        :selected-option="selectedOption"
        @select="handleSelect"
      />

      <ResultMessage
        :show="showResult"
        :result-type="resultType"
        :result-text="resultText"
        :score="score"
        :total="filteredQuestions.length"
        :is-complete="isComplete"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import QuestionCard from "../components/QuestionCard.vue";
import ResultMessage from "../components/ResultMessage.vue";

// 分类数据
const categories = ref(["book", "color", "sun", "fire", "food"]);
// 当前分类
const currentCategory = ref("color");

// 题库数据
const questions = ref([
  {
    word: "Apple",
    example: "I eat an apple every day",
    options: ["苹果", "香蕉", "橘子", "葡萄"],
    answer: "苹果",
    category: "food",
  },
  {
    word: "Dog",
    example: "The dog is barking loudly",
    options: ["猫", "狗", "兔子", "鸟"],
    answer: "狗",
    category: "animal",
  },
  {
    word: "Book",
    example: "I'm reading a book",
    options: ["书", "笔", "桌子", "电脑"],
    answer: "书",
    category: "book",
  },
  {
    word: "Color",
    example: "The artist used a vibrant color palette",
    options: ["黑色", "白色", "红色", "颜色"],
    answer: "颜色",
    category: "color",
  },
  {
    word: "Sun",
    example: "The sun is shining brightly",
    options: ["月亮", "太阳", "星星", "云朵"],
    answer: "太阳",
    category: "sun",
  },
  {
    word: "Fire",
    example: "The fire is burning warmly",
    options: ["水", "火", "土", "风"],
    answer: "火",
    category: "fire",
  },
]);

// 当前题目索引
const currentIndex = ref(0);
// 用户选择的选项
const selectedOption = ref(null);
// 得分
const score = ref(0);
// 是否显示结果
const showResult = ref(false);
// 结果类型（正确/错误）
const resultType = ref("");
// 结果文本
const resultText = ref("");

// 根据当前分类过滤的题目
const filteredQuestions = computed(() => {
  return questions.value.filter((q) => q.category === currentCategory.value);
});

// 当前题目
const currentQuestion = computed(() => {
  return filteredQuestions.value[currentIndex.value];
});

// 是否完成所有题目
const isComplete = computed(() => {
  return currentIndex.value >= filteredQuestions.value.length;
});

// 处理用户选择
const handleSelect = (option) => {
  selectedOption.value = option;
};

// 选择分类
const selectCategory = (category) => {
  currentCategory.value = category;
  currentIndex.value = 0;
  selectedOption.value = null;
  showResult.value = false;
  resultType.value = "";
  resultText.value = "";
  score.value = 0;
};

// 监听用户选择
watch(selectedOption, (newOption) => {
  if (newOption) {
    const isCorrect = newOption === currentQuestion.value.answer;
    resultType.value = isCorrect ? "correct" : "wrong";
    resultText.value = isCorrect ? "回答正确！" : "回答错误！";

    if (isCorrect && !isComplete.value) {
      score.value++;
    }

    showResult.value = true;
  }
});

// 监听当前题目索引变化
watch(currentIndex, () => {
  selectedOption.value = null;
  showResult.value = false;
  resultType.value = "";
  resultText.value = "";
});

// 下一题
const nextQuestion = () => {
  if (isComplete.value) {
    // 重新开始
    currentIndex.value = 0;
    score.value = 0;
    selectedOption.value = null;
    showResult.value = false;
  } else {
    // 下一题
    currentIndex.value++;
  }
};
</script>

<style scoped>
.app {
  min-height: 100vh;
  background-color: #f5f5f5;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  width: 100%;
  max-width: 500px;
}

.title {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin: 0;
}

.category-buttons {
  display: flex;
  gap: 8px;
  background-color: #f0f0f0;
  padding: 8px;
  border-radius: 8px;
  margin-bottom: 8px;
  flex-wrap: wrap;
  justify-content: center;
}

.category-btn {
  padding: 6px 12px;
  border: none;
  border-radius: 16px;
  background-color: white;
  font-size: 12px;
  cursor: pointer;
  transition: all 0.2s ease;
  color: #666;
}

.category-btn.active {
  background-color: #4caf50;
  color: white;
}

.progress {
  font-size: 14px;
  color: #666;
  margin-bottom: 8px;
}

@media (max-width: 768px) {
  .app {
    padding: 10px;
  }

  .title {
    font-size: 20px;
  }

  .category-buttons {
    gap: 6px;
    padding: 6px;
  }

  .category-btn {
    padding: 4px 10px;
    font-size: 10px;
  }
}
</style>