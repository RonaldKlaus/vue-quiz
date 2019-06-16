<template>
  <div class="question-box-container">
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
          :class="answerClass(index)"
          :disabled="answered"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"

      >
        Submit
      </b-button>
      <b-button
        @click="incrementIndex"
        variant="success"
        :disabled="!answered"
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<!--
  To make he currentQuestion useable we have to pass it via JS like this.
 -->
<script>
  import _ from "lodash"

  export default {
    props: {
      currentQuestion: Object,
      incrementIndex: Function,
      incrementCorrectCounter: Function
    },
    data() {
      return {
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false
      }
    },
    watch: {
      currentQuestion: {
        immediate: true, // calls the handler also on first occurence
        handler() {
          this.selectedIndex = null
          this.correctIndex = null
          this.answered = false
          this.shuffleAnswers()
        }
      }
    },
    computed: {
      answers() {
        // [...array] makes a copy of an array
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)

        return answers
      }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      shuffleAnswers() {
        let correctAnswer = this.currentQuestion.correct_answer
        let answers = [...this.currentQuestion.incorrect_answers, correctAnswer]

        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(correctAnswer)
      },
      submitAnswer() {
        let isCorrect = false

        if(this.selectedIndex == this.correctIndex) {
          isCorrect = true
        }

        this.answered = true
        this.incrementCorrectCounter(isCorrect)
      },
      answerClass(index) {
        let answerClass = ""

        if (!this.answered && this.selectedIndex == index) {
          answerClass = 'selected-answer'
        } else if (this.answered && this.correctIndex == index) {
          answerClass = 'correct-answer'
        } else if (
          this.answered &&
          this.correctIndex != this.selectedIndex &&
          this.selectedIndex == index
        ) {
          answerClass = 'wrong-answer'
        }

        return answerClass
      }
    }
  }
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px
  }

  .list-group-item:hover {
    background-color: #EEE;
    cursor: pointer;
  }

  .btn {
    margin: 0 5px
  }

  .selected-answer {
    background-color: lightblue;
  }

  .correct-answer {
    background-color: green;
    color: white;
  }

  .wrong-answer {
    background-color: red;
    color: black;
  }

</style>
