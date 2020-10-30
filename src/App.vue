<template>
  <div>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Header :numCorrect="numCorrect" :numTotal="numTotal" />
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextQuestion="nextQuestion"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import QuestionBox from "./components/QuestionBox";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      qApiUrl:
        "https://opentdb.com/api.php?amount=10&category=27&type=multiple",
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  mounted: function () {
    fetch(this.qApiUrl)
      .then((res) => res.json())
      .then((res) => (this.questions = res.results));
  },
  methods: {
    nextQuestion() {
      if (this.index <= 9) {
        this.index++;
      }
    },
    increment(isCorrect) {
      if (isCorrect === true) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
};
</script>

<style>
</style>
