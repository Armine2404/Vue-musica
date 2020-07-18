<template>
  <div class = "question-box-container">
  <b-jumbotron >

    <template v-slot:lead>
   {{ currentQuestion.question }}
    </template>

    <hr class="my-4">
    <b-list-group class= "list-group">
  <b-list-group-item
   v-for = "(answer , index ) in answers"
     :key = "index"
     @click = "selectAnswer(index)"
     :class= "[
         !answered && selectedIndex === index ? 'selected' :
         answered && correctIndex === index ? 'correct' : 
         answered && selectedIndex === index &&  correctIndex !== index ?
         'incorrect' : ''
          ]"> 
   {{answer}}
   </b-list-group-item>

</b-list-group >

    <b-button 
    class = "btn"
    variant="primary"
    @click = "submitAnswer"
    :disabled = "selectedIndex === null || answered"
    >
    Answer
    </b-button>
    <b-button variant="success" href="#" @click = "next">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
    props:{
        currentQuestion: Object,
        next:Function,
        increment:Function
    },
    data(){
        return{
            selectedIndex:null,
            correctIndex:null,
            shuffledAnswers:[],
            answered: false
        }
    },
    computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  watch:{
      currentQuestion:{
          inmediate:true,
          handler(){
            this.selectedIndex = null
            this.answered = false
            this.shuffleAnswers()
          }      
      }
  },
  methods:{
      selectAnswer(index){
          this.selectedIndex = index  
      },
      submitAnswer(){
          let isCorrect = false

          if(this.selectedIndex === this.correctIndex){
              isCorrect = true
          }
          this.answered = true
          this.increment(isCorrect)
     
      },
      shuffleAnswers(){
          let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answers ]
          this.shuffledAnswers = _.shuffle(answers)
          this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answers )
    
      }
    
  }

}
</script>

<style scoped>

.list-group{
    margin-bottom:10px
}

.list-group-item:hover{
   background:#EEE;
   cursor:pointer 
    }
.selected{
    background:lightblue;
}
.correct{
    background:lightgreen;
}
.incorrect{
    background:red;
}
.btn{
    margin-right:5px
}

</style>