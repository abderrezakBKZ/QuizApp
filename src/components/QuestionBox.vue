<template>
<div>
  <b-jumbotron >
    <template #header></template>

    <template #lead>
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

            <b-list-group>
            
                <b-list-group-item v-for="(answer,index) in shuffledAnswers" :key="index" @click="selectAnswer(index)"
                :class=" answerClass (index)">
                 {{answer}} 
                 
                </b-list-group-item>
            </b-list-group>
    

    <b-button variant="primary" @click ='submitAnswer' :disabled ="selectedIndex===null || answered" >
    Submit</b-button>
    <b-button @click="next" variant="success" href="#" v-if ='unlock' >Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'
export default {
    props : {
        currentQuestion : Object,
        next : Function,
        increment: Function
        
    },
    
watch : {
    currentQuestion: {
        immediate: true ,
        handler () {
            this.selectedIndex = null
            this.shuffleAnswers () ;
            this.answered = false; 
            this.unlock = false  

        }
    }
},




    data () {
        return {
            selectedIndex: null,
            shuffledAnswers : [],
            answered : false ,
            unlock : false
        }
    },
methods: {
    selectAnswer (index) {
        this.selectedIndex = index
        

    },
    answerClass (index) {
        let answerClass = ''

        if  (!this.answered && this.selectedIndex === index) {
            answerClass = 'selected'
        }
        else if (this.answered && this.correctIndex === index) {
            answerClass = 'correct' 
        }
        else if (this.answered && this.selectedIndex === index && this.correctIndex !== index ) {
            answerClass = 'incorrect'
        } 
        return answerClass
    },
    shuffleAnswers () {
        let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle (answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },

    submitAnswer() {
      let isCorrect = false
      if (this.selectedIndex === this.correctIndex ) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
      this.unlock = true
    },

},



    computed: {
    answers () {
       let answers = [...this.currentQuestion.incorrect_answers]
        answers.push (this.currentQuestion.correct_answer)
        return answers
       
     }
}
}



</script>


<style scoped>
.list-group {
    margin-bottom : 15px;
}
.list-group-item:hover {
    background:  #EEE;
    cursor : pointer ;
}
.btn {
    margin : 0 5px;
}

.selected {
    background-color : lightblue ;
}

.correct {
    background-color : lightgreen ;
}
.incorrect {
    background-color : red ;
}

</style>