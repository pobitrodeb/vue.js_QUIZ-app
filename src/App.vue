<script  setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    question: "What is vue.js ?",
    answer: 0,
    options: ["A Frontend Framework", "A Backend Framework", "API generator"],
    seleted: null,
  },
  {
    question: "What is Laravel ?",
    answer: 1,
    options: ["A Frontend Framework", "A Backend Framework", "API generator"],
    seleted: null,
  },
  {
    question: "What is Node.js ?",
    answer: 2,
    options: ["A Frontend Framework", "API generator", "A Backend Database"],
    seleted: null,
  },
  {
    question: "What is PHP ?",
    answer: 1,
    options: [
      "A Frontend Framework",
      "Server Site Language",
      "A Backend Database",
    ],
    seleted: null,
  },
  {
    question: "How was the app? ?",
    answer: 2,
    options: ["Good", "Better", "Usual"],
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
  <section v-if="!quizCompleted" class="py-5">
    <div class="container">
      <div class="card">
        <div
          class="card-header bg-primary text-white d-flex justify-content-between"
        >
          <div class="questions">
            <h2>{{ getCurrentQuestion.question }}</h2>
          </div>
          <div class="btn bg-dark text-white text-center btn-md">
            <h3>Score {{ score }} / {{ questions.length }}</h3>
            
          </div>
        </div>

        <div class="card-body">
          <div class="options">
            <label
              v-for="(option, index) in getCurrentQuestion.options"
              :key="index"
              :class="`option${
                getCurrentQuestion.seleted == index
                  ? index == getCurrentQuestion.answer
                    ? 'correct'
                    : 'wrong'
                  : ''
              } ${
                getCurrentQuestion.seleted != null &&
                index != getCurrentQuestion.seleted
                  ? 'disable'
                  : ''
              }`"
            >
              <input
                type="radio"
                :name="getCurrentQuestion.index"
                :value="index"
                v-model="getCurrentQuestion.seleted"
                :disabled="getCurrentQuestion.seleted"
                @change="SetAnswer"
              />
              {{ option }}
            </label>
          </div>
        </div>

        <div class="card-footer bg-primary text-white">
          <button
            @click="NextQuesttion"
            :disabled="!getCurrentQuestion.seleted"
            class="btn btn-primary w-100"
          >
            <h5>
              {{
                getCurrentQuestion.index == questions.length - 1
                  ? "Finish"
                  : getCurrentQuestion.seleted == null
                  ? "Select an option"
                  : "Next Question => "
              }}
            </h5>
          </button>
         
        </div>
      </div>
      <p class="text-center m-3">
            Quiz App with Vue.js || © 2023 Copyright: www.pobitrodeb.com
      </p>
    </div>
  </section>

  <!-- Ouput Section  -->
  <section v-else class="py-5">
    <div class="container">
      <div class="card">
        <div class="card-header bg-primary text-white text-center">
          <h3>You have a finished quiz</h3>
        </div>
        <div class="card-body text-center">
          <!-- <p>Your score is {{ score }} / {{ questions.length }}</p> -->
          <h3>Total Question: {{ questions.length }}</h3>
          <h3>Right Answer: {{ score }}</h3>
        </div>
      </div>
    </div>
  </section>
</template>



<style scoped>
.questions {
  margin: 1rem;
}
.options {
  margin: 1rem;
}
.option {
  background-color: #37294b;
  color: white;
  width: 100%;
  padding: 15px;
  margin: 1rem;
  font-size: 18px;
  font-weight: 600;
  border-radius: 50px;
}
.option:hover {
  background-color: #fff;
  color: #37294b;
  border: 1px solid;
  padding: 10px;
  box-shadow: 2px 6px #888888;
}
.option.correct {
  background-color: green;
  color: white;
}
.option.wrong {
  background-color: red;
  color: white;
}
</style>