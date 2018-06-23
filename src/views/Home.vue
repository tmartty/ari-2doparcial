<template>
  <div class="questions-container" id="examen">
    <button @click="startExam()">Examen con todas las preguntas</button>
    <button @click="startExam(40)">Examen con 40 preguntas</button>
    <div v-for="question, index in questions" class="question"
      :class="{ incorrect: showResults && question.answer != null && question.answer != question.correctAnswer,
      correct: showResults && question.answer != null && question.answer == question.correctAnswer  }">
      <div class="text">
        <b>{{index + 1}}.</b> 
        {{question.text}}
      </div>
      <div class="option">
        <input type="radio" :id="index+'TRUE'" :value="true" v-model="question.answer">
        <label :for="index+'TRUE'"> Verdadero</label>
      </div>
      <div class="option">
        <input type="radio" :id="index+'FALSE'" :value="false" v-model="question.answer">
        <label :for="index+'FALSE'"> Falso</label>
      </div>
      <div class="option">
        <input type="radio" :id="index+'NULL'" :value="null" v-model="question.answer">
        <label :for="index+'NULL'"> No se</label>
      </div>
    </div>
    <button id="resultados" @click="showResults = !showResults">Mostrar resultados</button>
    <div class="results" v-if="showResults">
      Correctas: {{totalCorrect}}
      <br>
      Incorrectas: {{totalIncorrect}}
      <br>
      Sin contestar: {{totalNotAnswered}}
      <br>
      {{ result }}/{{questions.length}}
    </div>
    <div class="credits">
      Made with ♥ by Tomas Martty
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import questions from '@/questions.js'

export default {
  name: 'home',
  data() {
    return {
      showResults: false,
      totalCorrect: 0,
      totalIncorrect: 0,
      totalNotAnswered: 0,
      questions: []
    }
  },
  computed: {
    result: function() {
      var correct = 0
      this.totalCorrect = 0
      this.totalIncorrect = 0
      this.totalNotAnswered = 0
      this.questions.forEach((question) => {
        if (question.answer != null && question.correctAnswer == question.answer) {
          this.totalCorrect++
          correct++
        } else if (question.answer != null && question.correctAnswer != question.answer) {
          this.totalIncorrect++
          correct--
        } else if (question.answer == null) {
          this.totalNotAnswered++
        }
      })
      return correct
    }
  },
  mounted() {
    this.questions = questions
  },
  methods: {
    startExam(maxQuestions) {
      if (maxQuestions) {
        this.questions = this.getRandom(questions, maxQuestions)
      } else {
        this.questions = this.getRandom(questions, questions.length)
      }
      this.questions.forEach((question)=> {
        question.answer = null
      })
      this.totalCorrect = 0
      this.totalIncorrect = 0
      this.totalNotAnswered = 0
      this.showResults = false
    },
    getRandom(arr, n) {
      var result = new Array(n)
      var len = arr.length
      var taken = new Array(len)
      if (n > len)
        throw new RangeError("getRandom: more elements taken than available");
      while (n--) {
        var x = Math.floor(Math.random() * len);
        result[n] = arr[x in taken ? taken[x] : x];
        taken[x] = --len in taken ? taken[len] : len;
      }
      return result;
    }
  }
}
</script>

<style scoped>
  .questions-container {
    text-align: left;
  }
  button {
    margin: 5px;
    padding: 5px;
  }
  button:hover {
    cursor: pointer;
    opacity: 0.8;
  }
  .question {
    background-color: rgba(0,0,0,0.05);
    padding: 10px;
    margin: 10px 0;
    border-radius: 5px;
  }
  .question .text {
    margin-bottom: 10px;
  }
  .option, label {
    display: flex;
    width: 100%;
  }
  .incorrect {
    color: white;
    background-color: rgba(255,0,0,1);
  }
  .correct {
    color: white;
    background-color: rgba(0,155,0,1);
  }
  .credits {
    text-align: right;
    opacity: 0.2;
    text-transform: uppercase;
    font-size: 12px;
    margin: 5% 0;
    transition: all 0.4s;
  }
  .credits:hover {
    opacity: 1;
    cursor: default;
  }
</style>