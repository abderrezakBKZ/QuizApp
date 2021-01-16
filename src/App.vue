<template>
  <div id="app">
    
    <tete
    :numCorrect="numCorrect"
    :numTotal="numTotal" />

  <b-container class="bv-example-row">
  <b-row>
    <b-col sm="6" offset = "3">
    <QuestionBox 
    v-if ="Questions.length"
    :currentQuestion =" Questions[index]"
    :next ="next"
    :increment="increment" />
    </b-col>
    
    
  </b-row>
</b-container>

    
  </div>
</template>

<script>
import tete from './components/tete.vue'
import QuestionBox from './components/QuestionBox.vue'
export default {
  name: 'App',
  components: {
    tete,
    QuestionBox
  },
  
  data () {
    return {
      Questions : [],
      index : 0,
      numCorrect : 0,
      numTotal : 0
    }
  }, 
   methods: {
     next () {
       this.index++
     },
      increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
   },

   

  mounted() {
    fetch ('https://opentdb.com/api.php?amount=10&category=22&type=multiple', {
      method: 'get'
    })
    .then ((response) => {
      return (response.json())
    })
    .then ((jsonData) => {
      this.Questions = jsonData.results
    })
  },
}
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
