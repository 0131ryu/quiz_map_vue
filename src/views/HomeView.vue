<template>
  <main class="flex h-screen items-center justify-center bg-gray-100">
    <div
      class="container overflow-hidden bg-white flex-none relative shadow-lg rounded-lg px-12 py-6"
    >
      <div class="relative z-20">
        <div class="text-right text-gray-800">
          <p class="text-sm leading-3">Score</p>
          <p class="font-bold">100</p>
        </div>
        <div class="bg-white shadow-lg p-1 rounded-full w-full h-5 mt-4">
          <div
            class="bg-yellow-400 rounded-full w-11/12 h-full"
            :style="`width:100%`"
          ></div>
        </div>
      </div>
      <!-- score, timer -->

      <div
        class="rounded-lg bg-gray-100 p-2 boxShadow text-center font-bold text-gray-800 mt-10"
      >
        <div class="bg-white p-5">What do you think about Vue?</div>
      </div>
      <!-- question -->
      <div class="mt-8">
        <div v-for="(choice, item) in currentQuestion.choices" :key="item">
          <div
            class="boxShadow option-default bg-gray-100 p-2 rounded-lg mb-3 relative"
            :ref="optionChosen"
            @click="onOptionClicked(choice, item)"
          >
            <div
              class="bg-yellow-400 p-1 transform rotate-45 rounded-md h-10 w-10 text-black font-bold absolute right-0 top-0 shadow-md"
            >
              <p class="transform -rotate-45">+10</p>
            </div>
            <!-- correct result  +10 -->
            <div class="rounded-lg font-bold flex p-2">
              <div class="p-1 rounded-lg">{{ item }}</div>
              <div class="flex items-center pl-6">{{ choice }}</div>
            </div>
          </div>
        </div>
      </div>
      <!-- answer -->
      <div class="mt-8 text-center">
        <div class="h-1 w-12 bg-gray-800 rounded-full mx-auto"></div>
        <p class="font-bold text-black">1/10</p>
      </div>
    </div>
  </main>
</template>

<style scoped>
.boxShadow {
  box-shadow: 6px 6px 18px rgba(0, 0, 0, 0.09), -6px -6px 18px #ffffff;
}
.container {
  max-width: 400px;
  border-radius: 25px;
}
</style>

<script>
import { onMounted, ref } from "vue";

export default {
  setup() {
    let isClick = true;
    let questionCounter = ref(0);
    let score = ref(0);
    const currentQuestion = ref({
      question: "",
      answer: 1,
      choices: [],
    });
    const questions = [
      {
        question:
          "Which programming language shares its name with an island in Indonesia?",
        answer: 1,
        choices: ["Java", "Python", "C", "Jakarta"],
      },
      {
        question: "On Twitter, what is the character limit for a Tweet?",
        answer: 3,
        choices: ["120", "160", "140", "100"],
      },
      {
        question: "What does the 'MP' stand for in MP3?",
        answer: 4,
        choices: [
          "Music Player",
          "Multi Pass",
          "Micro Point",
          "Moving Picture",
        ],
      },
    ];
    const onStart = () => {
      currentQuestion.value = questions[questionCounter.value];
      console.log("currentQuestion", currentQuestion);
    };
    const loadQuestion = () => {
      isClick = true;
      if (questions.length > questionCounter.value) {
        currentQuestion.value = questions[questionCounter.value];
        console.log("Current questions", currentQuestion.value);
        questionCounter.value++;
        console.log("currentQuestion.choices", currentQuestion.value.choices);
      } else {
        console.log("Out of questions");
      }
    };
    let itemsRef = [];
    const optionChosen = (element) => {
      if (element) {
        itemsRef.push(element);
      }
    };
    const onOptionClicked = (choice, item) => {
      if (isClick) {
        const divContainer = itemsRef[item];
        const optionId = item + 1;
        if (currentQuestion.value.answer === optionId) {
          console.log("you are correct");
          divContainer.classList.add("option-correct");
          divContainer.classList.remove("option-default");
          score.value += 10;
        } else {
          console.log("you are wrong");
          divContainer.classList.add("option-wrong");
          divContainer.classList.remove("option-default");
        }
        console.log("choice", choice);
        console.log("item", item);
      } else {
        console.log("Out of questions");
      }
    };
    onMounted(() => {
      onStart();
      loadQuestion();
    });
    return {
      questions,
      currentQuestion,
      questionCounter,
      onStart,
      optionChosen,
      onOptionClicked,
    };
  },
};
</script>
