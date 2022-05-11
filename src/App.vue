<script setup>
import { ref, computed, onBeforeMount, onMounted } from "vue";
import _questionBlocks from "./data/questionBlocks";
import Answer from "./components/Answer.vue";
import QuizForm from "./components/QuizForm.vue";
import QuizFinished from "./components/QuizFinished.vue";

const currentQuestionPosition = ref(0);
const quizPoints = ref(0);
const quizFinished = ref(false);
const questionBlocks = ref(_questionBlocks);
const quizFormVisibility = ref(false);

onBeforeMount(() => {
  shuffleQuestions();
  shuffleAnswers(); // CHECK THIS LATER
});

const shuffleQuestions = () => {
  questionBlocks.value.sort(() => Math.random() - 0.5);
};

const shuffleAnswers = () => {
  questionBlocks.value.forEach((block) => {
    block.answers.sort(() => Math.random() - 0.5);
  });
};

const currentQuestion = computed(() => {
  return questionBlocks.value[currentQuestionPosition.value].question;
});

const restartQuiz = () => {
  quizFinished.value = false;
  quizPoints.value = 0;
  currentQuestionPosition.value = 0;
  shuffleQuestions();
  shuffleAnswers();
};

const finishQuiz = () => {
  quizFinished.value = true;
};

const progressForward = () => {
  if (questionBlocks.value.length === currentQuestionPosition.value + 1) {
    finishQuiz();
    return;
  }

  currentQuestionPosition.value++;
};

const handleAnswerClick = ({ answer, position }) => {
  if (questionBlocks.value[position].correctAnswer === answer) {
    quizPoints.value++;
  }

  progressForward();
};

const handleAddQuestionBlock = ({
  customQuestion,
  customAnswer,
  customAnswers,
}) => {
  const newQuestionBlock = {
    id: questionBlocks.value[questionBlocks.value.length - 1].id++,
    question: customQuestion,
    correctAnswer: customAnswer,
    answers: customAnswers.split(","),
  };

  questionBlocks.value.push(newQuestionBlock);
  restartQuiz();
};
</script>

<template>
  <h1 class="title">Vue quiz app</h1>
  <h2 class="points">Total points: {{ quizPoints }}</h2>
  <h3 class="question">{{ currentQuestion }}</h3>
  <div class="answers-container">
    <Answer
      :data="questionBlocks"
      :position="currentQuestionPosition"
      @answerClick="(payload) => handleAnswerClick(payload)"
    />
  </div>
  <QuizFinished :finished="quizFinished" @restart="restartQuiz" />

  <div class="quiz-form-container">
    <div v-if="quizFormVisibility">
      <QuizForm
        @addQuestionBlock="(payload) => handleAddQuestionBlock(payload)"
        @hideForm="quizFormVisibility = false"
      />
    </div>
    <button
      class="btn toggle-form"
      @click="quizFormVisibility = !quizFormVisibility"
      v-else
    >
      Show form
    </button>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  width: 80%;
  max-width: 550px;
  margin: auto;
  user-select: none;
  background: linear-gradient(45deg, #17225c, #2b998e);
  min-height: 100vh;
  background-repeat: no-repeat;
  color: white;
}

.title {
  text-align: center;
  text-transform: uppercase;
  font-weight: 200;
}

.points {
  text-align: center;
  text-transform: lowercase;
  font-weight: 200;
}

.answers-container {
  list-style: none;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}

.question {
  background: rgba(255, 255, 255, 0.2);
  padding: 1rem;
  border-radius: 20px;
}

* {
  font-family: "Arial";
}

.btn {
  border-radius: 10px;
  outline: none;
  border: none;
  padding: 1rem 2rem;
  font-size: 1.1rem;
}

.btn:hover {
  cursor: pointer;
}

.toggle-form {
  position: fixed;
  text-transform: uppercase;
  font-weight: bold;
  bottom: 25px;
  left: 50%;
  transform: translate(-50%, 0);
  font-weight: thin;
  background: linear-gradient(45deg, deeppink, #aa00cc);
  color: white;
  font-size: 1.2rem;
  transition: all 0.2s ease;
}

.toggle-form:hover {
  opacity: 0.8;
}
</style>
