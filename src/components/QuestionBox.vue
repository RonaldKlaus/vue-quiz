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
          :class="[selectedIndex == index ? 'selected-answer' : '']"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="incrementIndex" variant="success" href="#">
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
      incrementIndex: Function
    },
    data() {
      return {
        selectedIndex: null,
        shuffledAnswers: []
      }
    },
    watch: {
      currentQuestion: {
        immediate: true, // calls the handler also on first occurence
        handler() {
          this.selectedIndex = null
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
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
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
  }

  .correct-answer {
    background-color: red;
  }

</style>
