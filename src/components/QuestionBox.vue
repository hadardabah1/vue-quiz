<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ CurrentQquestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[
          !answered && selectedIndex === index ? 'selected' : 
          answered && correctIndex === index ? 'correct' : 
          answered && selectedIndex===index && correctIndex !== index ? 'incorrect' : 
          ''
          ]"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button 
      variant="primary"
       @click="submitAnswer"
       :disabled="selectedIndex===null || answered"
       >
        Submit
      </b-button>
      <b-button @click="NextQuestion" variant="success" href="#"
        >Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    CurrentQquestion: Object,
    NextQuestion: Function,
    increment: Function,
  },

  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered:false
    };
  },
  computed: {
    answers() {
      let answers = [...this.CurrentQquestion.incorrect_answers];
      answers.push(this.CurrentQquestion.correct_answer);
      return answers;
    },
  },

  watch: {
    CurrentQquestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered= false
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log(index);
    },
    shuffleAnswers() {
      let answers = [
        ...this.CurrentQquestion.incorrect_answers,
        this.CurrentQquestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex=this.shuffledAnswers.indexOf(this.CurrentQquestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect = false;
      console.log(this.answers[3]);
      console.log(this.shuffledAnswers[this.selectedIndex]);
      if (this.shuffledAnswers[this.selectedIndex] == this.answers[3]) {
        isCorrect = true;

        console.log(isCorrect);
      }

       this.answered = true

      this.increment(isCorrect);
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: lightcoral;
}
</style>
