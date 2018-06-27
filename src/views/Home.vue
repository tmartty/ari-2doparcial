<template>
  <div class="questions-container" id="examen">
    <br>
    <h3>Elegir temas</h3>
    <div class="subjects">
      <div class="subject" v-for="subject, index in allSubjects">
        <input :id="index" type="checkbox" name="vehicle" :value="subject.questions" v-model="pickedSubjects">
        <label :for="index">{{subject.subject}}</label>
      </div>
    </div>
    <br>
    <h3>Elegir cantidad de preguntas</h3>
    <div class="options">
      <button class="btn" @click="startExam()" :class="{active: this.questions.length != 50}">Todas las preguntas</button>
      <button class="btn" @click="startExam(50)" :class="{active: this.questions.length == 50}">50 preguntas</button>
    </div>
    <div v-for="question, index in questions" class="question"
      :class="{ incorrect: showResults && question.answer != null && question.answer != question.correctAnswer,
      correct: showResults && question.answer != null && question.answer == question.correctAnswer  }">
      <div class="text">
        <b>{{index +1}}.</b> 
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
    <br>
    <hr>
    <button id="resultados" @click="showResults = !showResults">Mirá los resultados</button>
    <div class="results" v-if="showResults">
      <div class="result">
        Correctas: {{totalCorrect}}
      </div>
      <div class="result">
        Incorrectas: {{totalIncorrect}}
      </div>
      <div class="result">
        Sin contestar: {{totalNotAnswered}}
      </div>
      <div class="result" :class="{ incorrect: result < 30, correct: result >= 30 }">
        Resultado: {{ result }}/{{questions.length}}
      </div>
    </div>
    <div class="credits">
      Made with ♥ by Tomas Martty
    </div>
  </div>
</template>

<script>
import questions_ordered_by_subject from '@/questions_ordered_by_subject.js'

export default {
  name: 'home',
  data() {
    return {
      showResults: false,
      totalCorrect: 0,
      totalIncorrect: 0,
      totalNotAnswered: 0,
      questions: [],
      // allQuestions: [],
      allSubjects: [],
      pickedSubjects: []
    }
  },
  computed: {
    allQuestions: function() {
      var array = []
      this.pickedSubjects.forEach((subject)=>{
        subject.forEach((question)=>{
          array.push(question)
        })
      })
      return array
    },
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
    this.allSubjects = questions_ordered_by_subject.sort(function(a, b){
      if(a.subject < b.subject) return -1;
      if(a.subject > b.subject) return 1;
      return 0;
    })
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
  .subjects {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }
  .subjects .subject {
    width: 49%;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    align-items: center;
    font-size: 12px;
    border: 1px solid rgba(0,0,0,0.05);
  }
  .subjects .subject label {
    align-items: center;
    display: flex;
    padding: 5px 0px;
    height: 100%;
  }
  .subjects .subject:hover {
    cursor: pointer;
    background: rgba(0,0,0,0.05);
  }
  #resultados {
    text-transform: uppercase;
    width: 100%;
    margin: 10px 0;
    font-size: 18px;
  }
  .results {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .result {
    text-align: center;
    border: 1px solid #CCC;
    width: 45%;
    padding: 10px;
    margin: 10px;
    text-transform: uppercase;
  }
</style>