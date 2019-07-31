<template>
  <div>
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#"
        @click="submitAnswer">
        Submit
      </b-button>
      <b-button @click="nextQuestion" variant="success" href="#">Next Question</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
  import _ from 'lodash';

  export default {
    props: {
      currentQuestion: Object,
      nextQuestion: Function,
      increment: Function
    },

    data(){
      return{
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: []
      }
    },

    computed: {
      answers(){
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return answers
      }
    },

    watch: {
      currentQuestion: {
        immediate: true,
        handler(){
          this.selectedIndex = null
          this.shuffleAnswers();
        }
      }
    },
    methods: {
      selectAnswer(index){
        this.selectedIndex = index
      },

      submitAnswer(){
        let isCorrect = false
        if (this.selectedIndex === this.correctIndex){
          isCorrect = true
        }

        this.increment(isCorrect)
      },

      shuffleAnswers(){
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        console.log(this.shuffledAnswers)
        console.log(this.correctIndex)
      }
    }
  }
</script>

<style scoped>

  .list-group{
    margin-bottom: 15px;
  }

  .list-group-item:hover{
    background: #EEE;
    cursor: pointer;
  }

  .btn{
    margin: 0 8px;
  }

  .selected {
    background-color: lightblue;
  }

  .selected:hover{
    background: lightblue;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect{
    background-color: red;
  }

</style>
