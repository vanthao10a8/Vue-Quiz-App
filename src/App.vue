<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal"></Header>
    <b-container class="bv-example-row">
        <b-col></b-col>
        <b-col>
          <QuestionBox 
            v-if="questions.length" 
            :currentQuestion="questions[index]" 
            :next="next"
            :disableNextFlag="disableNextFlag"
            :increment="increment"></QuestionBox>
        </b-col>
        <b-col></b-col>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      disableNextFlag : false,
      numCorrect : 0,
      numTotal : 0
    };
  },
  methods: {
    next: function() {
      if(this.questions.length - 1 > this.index)
        this.index++;
      else 
        this.disableNextFlag = true;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10", {
      method: "get"
    })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        return (this.questions = jsonData.results);
      });
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
