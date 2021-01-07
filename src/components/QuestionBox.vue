<template>        
   <div calss="question-box-container">
        <b-jumbotron class="quest-container">
            <template v-slot:lead >
            {{currentQuestion.question}}
            </template>
            <hr class="my-4">
            <b-list-group>
           
                <b-list-group-item 
                class="answer"  
                :key="index" 
                v-for="(answer, index) in answers" 
                @click="!answered && selectAnswer(index)"
                :class='answerClass(index)'
                >
                 {{ shuffledAnswers[index] }}</b-list-group-item>
           
            </b-list-group>
            <div class="button-container">
           
            <b-button   class="button submit"  variant="primary" 
            @click="submitAnswer" 
            :disabled="selectedIndex == null || answered"
            v-if="!answered"
            >Submit
            </b-button>
           
            <b-button class="button next" variant="success" 
            v-if="answered && questIndex <= questions.length - 2" 
            @click="next"
            >Next</b-button>

            <b-button class="button restart" variant="danger"
             v-if="answered && questIndex == questions.length - 1" 
            @click="restart">Replay</b-button>

                    </div>
        </b-jumbotron>
</div>
</template>
<script>
import _ from "lodash" 

export default {
    name:"QuestionBox",
    props:{
        currentQuestion : Object,
        next : Function,
        increment: Function,
        questIndex: Number,  
        questions: Array  
    },
    data(){
        return{
            selectedIndex: null,
            shuffledAnswers : [],
            correctIndex: null,
            answered: null, 
        }
    },
    computed:{
        answers(){

            let answers  = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch: {
        currentQuestion :{
            immediate: true , 
            handler(){
                this.selectedIndex = null                
                this.answered = null
                this.shuffleAnswers()
                this.decodeHtml()
            }
        }  
        },
    methods :{ 
        restart(){
            document.location.reload()
        },
        decodeHtml(){
            let shuffledAnswersEsc = []
            this.currentQuestion.question = _.unescape(this.currentQuestion.question)
            this.currentQuestion.question  = this.currentQuestion.question.replaceAll("&#039;" , "'")
            for(let i=0 ; i < this.shuffledAnswers.length ; i++){
                shuffledAnswersEsc.push(_.unescape(this.shuffledAnswers[i].replaceAll("&#039;" , "'"))) 
            }
            this.shuffledAnswers = shuffledAnswersEsc
        },
        selectAnswer(index){
            this.selectedIndex = index
        },submitAnswer(){
            let isCorrect = false 
            if (this.selectedIndex == this.correctIndex){
                isCorrect = true 
            }
            this.answered = true
            this.increment(isCorrect)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass(index){
            let answerClass = ""
            if(!this.answered  && this.selectedIndex === index ){
             answerClass = "selected"
            }else if (this.answered  && this.correctIndex === index){
                answerClass = "correct"
            }else if(this.answered && this.selectedIndex === index && this.correctIndex != index){
                answerClass = "incorrect"
            }
            return answerClass
        }
    }
}
</script>
<style scoped>
    .answer{
        background-color:#f5f5f5;
        margin-bottom:8px;
        padding: 17px 20px;
        border-radius: 0.35em;
    }
    .answer:hover{
       background-color:  rgb(143 204 157);
       color: white;
       text-shadow: 1px 1px 1px rgba(0,0,0,0.1);
       cursor: pointer;
    }
     .answer.selected{
       background-color:  #49bb63;
       color: white;
        border-bottom: 5px solid #44ad5c;
        color: white;
       text-shadow: 1px 1px 1px rgba(0,0,0,0.1);
    }
    .answer.correct{
      background-color:  #49bb63;
      color: white;
      border-bottom: 5px solid #44ad5c;
      color: white;
      text-shadow: 1px 1px 1px rgba(0,0,0,0.1);
    }
    .answer.incorrect{
        background-color:  #da4856;
        color: white;
        border-bottom: 5px solid #b7515b;
        color: white;
        text-shadow: 1px 1px 1px rgba(0,0,0,0.1);
    }
    .button-container{
        margin-top:20px;
    }
    .submit{
        border-bottom: 5px solid rgb(31 85 144);
    }
    .next{
        border-bottom: 5px solid rgb(28 121 49);
    }
    .restart{
        border-bottom:5px solid #79464c
    }
    .quest-container{
        box-shadow: 1px 1px 1px rgba(0,0,0,0.3);
        border-radius: 0.35em;
    }
    .button{
        padding: 17px 20px;
        border-radius: 10px;
        font-size: 16px;
        font-weight: 900;
        min-width: 140px;
        margin-bottom: 5px;
        margin-right:5px;
    }
    #app > div.bv-example-row.container > div > div > div > div > p{
        color: #616161;
        FONT-WEIGHT: 700;
        font-size: 25px;
    }
    .list-group-item{
        font-size: 20px;
        color: #616161;
        font-weight: 500;
    }
    #app > div.bv-example-row.container > div > div > div > div{
        max-width: 600px;
        min-width: 500px;
        padding-bottom: 40px;
        padding-top: 50px;
    }
</style>