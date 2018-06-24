<template>
  <div class="questions-container" id="examen">
    <div class="options">
      <button class="btn" @click="selectExam(1)" :class="{active: currentParcial == 1}">1er Parcial</button>
      <button class="btn" @click="selectExam(2)" :class="{active: currentParcial == 2}">2do Parcial</button>
    </div>
    <div class="options">
      <button class="btn" @click="startExam()" :class="{active: this.questions.length != 50}">Todas las preguntas</button>
      <button class="btn" @click="startExam(50)" :class="{active: this.questions.length == 50}">50 preguntas</button>
    </div>
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
      <span :class="{ incorrect: result < 30, correct: result >= 30 }">
        Resultado: {{ result }}/{{questions.length}}
      </span>
    </div>
    <div class="credits">
      Made with ♥ by Tomas Martty
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import questions_1er_parcial from '@/questions_1er_parcial.js'
import questions_2do_parcial from '@/questions_2do_parcial.js'

export default {
  name: 'home',
  data() {
    return {
      showResults: false,
      totalCorrect: 0,
      totalIncorrect: 0,
      totalNotAnswered: 0,
      questions: [],
      allQuestions: [],
      currentParcial: 1
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
    this.allQuestions = questions_1er_parcial
    this.questions = this.allQuestions
  },
  methods: {
    startExam(maxQuestions) {
      if (maxQuestions) {
        this.questions = this.getRandom(this.allQuestions, maxQuestions)
      } else {
        this.questions = this.getRandom(this.allQuestions, this.allQuestions.length)
      }
      this.questions.forEach((question)=> {
        question.answer = null
      })
      this.totalCorrect = 0
      this.totalIncorrect = 0
      this.totalNotAnswered = 0
      this.showResults = false
    },
    selectExam(number) {
      if (number == 1) {
        this.allQuestions = questions_1er_parcial
        this.startExam(50)
        this.currentParcial = 1
        this.showResults = false
      }
      if (number == 2) {
        this.allQuestions = questions_2do_parcial
        this.startExam(50)
        this.currentParcial = 2
        this.showResults = false
      }
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
  .options {
    display: flex;
    width: 100%;
  }
  .options .btn {
    text-align: center;
    text-transform: uppercase;
    display: flex;
    justify-content: center;
    width: 100%;
    opacity: 0.2;
    background: rgba(0,0,0,0.2);
  }
  .options .btn.active {
    opacity: 1;
    background: rgba(255,255,255) !important;
  }
</style>