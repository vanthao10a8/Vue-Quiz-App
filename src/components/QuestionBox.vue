<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selecteAnswer(index)"
          :class="answerClass(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button 
        variant="primary"
        :disabled="selectedIndex === null || answered"
        @click="submitAnswer"
      >
        Submit
      </b-button>
      <b-button 
        v-on:click="next" 
        :disabled="disableNextFlag" 
        variant="success">
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    disableNextFlag : Boolean,
    increment : Function

  },
  data(){
    return {
      selectedIndex : null,
      shuffledAnswers : [],
      correctIndex : null,
      answered : false
    }
  },
  watch : {
    currentQuestion : {
      immediate : true,
      handler(){
        this.selectedIndex = null;
        this.shuffleAnswers();
      }
    }
  },
  computed : {
    answers(){
      let answer = [...this.currentQuestion.incorrect_answers];
      answer.push(this.currentQuestion.correct_answer);
      return answer;
    }
  },
  methods : {
    selecteAnswer(index){
      this.selectedIndex = index;
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      this.answered = false
    },
    submitAnswer(){
      let isCorrect = false
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },
    answerClass(index) {
      let answerClass = ''
      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = 'incorrect'
      }
      return answerClass
    }
  }
};
</script>

<style scoped>
.list-group{
  margin-bottom: 15px;
}
.list-group-item{
  margin: 5px;
}
.btn{
  margin: 0 5px;
}
.selected{
  background-color: lightblue;
}
.list-group-item:hover{
  background-color: lightblue;
}
.correct{
  background-color: lightgreen;
}
.incorrect{
  background-color: red;
}
</style>