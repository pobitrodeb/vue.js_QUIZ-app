<script  setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    question: "What is vue.js",
    answer: 0,
    options: ["A Frontend Framework", "A Backend Framework", "API generator"],
    seleted: null,
  },
  {
    question: "What is Laravel",
    answer: 1,
    options: ["A Frontend Framework", "A Backend Framework", "API generator"],
    seleted: null,
  },
  {
    question: "What is Node.js",
    answer: 2,
    options: ["A Frontend Framework", "API generator", "A Backend Database"],
    seleted: null,
  },
  {
    question: "What is PHP",
    answer: 1,
    options: ["A Frontend Framework", "Server Site Language", "A Backend Database"],
    seleted: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.seleted == q.answer) {
      value++;
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;

  return question;
});

const SetAnswer = (evt) => {
  questions.value[currentQuestion.value].seleted = evt.target.value;
  evt.target.value = null;
};

const NextQuesttion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <h2 class="bg-primary text-white text-center py-3">
    Quiz-App with Vue.js | www.pobitrodeb.com
  </h2>

<section v-if="!quizCompleted">
   <div class="container">
    <div class="card">
      <div class="card-body">

        <div class="quiz" >
          <div class="quiz-info">
            <span class="question">{{ getCurrentQuestion.question }}</span>
            <span class="score">
              Score {{ score }} / {{ questions.length }}
            </span>
          </div>
        </div>

        <div class="options">
         <label v-for="(option, index) in getCurrentQuestion.options" :key="index"
         :class="`option ${
            getCurrentQuestion.seleted == index ? index == getCurrentQuestion.answer ? 'correct' : 'wrong' : ''
         } ${
            getCurrentQuestion.seleted != null && index != getCurrentQuestion.seleted ? 'disable' : ''
         }`"
         >
            <input 
            type="radio" 
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.seleted"
            :disabled="getCurrentQuestion.seleted"
            @change="SetAnswer">
            {{ option }}
         </label>
        </div>
        <button @click="NextQuesttion" :disabled="!getCurrentQuestion.seleted">
         {{ getCurrentQuestion.index == questions.length -1 ? 'Finish' : getCurrentQuestion.seleted == null
               ? 'Select an option'
               : 'Next Question'
          }}
        </button>

      </div>
    </div>
  </div>
</section>

<section v-else>
   <h2>You have a finished quiz</h2>
   <p>Your score is {{ score }} / {{ questions.length }}</p>
</section>

</template>



<style scoped>
</style>