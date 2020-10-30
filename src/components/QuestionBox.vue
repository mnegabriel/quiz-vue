<template>
  <div class="question-box">
    <b-jumbotron>
      <template #lead>{{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :disabled="answered"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        :disabled="selectedIndex === null || answered"
        @click="submitAnswer"
        variant="primary"
        href="#"
        >Submit</b-button
      >
      <b-button @click="nextQuestion" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "QuestionBox",
  props: ["currentQuestion", "nextQuestion", "increment"],
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  computed: {
    },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(id) {
      this.selectedIndex = id;
    },
    shuffleAnswers() {
      let answerList = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      let answersShuffled = _.shuffle(answerList);
      this.shuffledAnswers = answersShuffled;
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      this.answered = true;
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.increment(isCorrect);
    },
    answerClass(index) {
      let itemClass = "";
      if (!this.answered && this.selectedIndex === index) {
        itemClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        itemClass = "correct";
      } else if (this.answered && this.selectedIndex === index) {
        itemClass = "wrong";
      }
      return itemClass;
    },
  },
};
</script>

<style scoped>
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.wrong {
  background-color: rgb(255, 154, 154);
}
</style>