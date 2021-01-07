<template>
  <div id="app">
    <meta charset="utf-8">
    <Header 
    :questIndex="questIndex" 
    :questions= "questions"
    :numTotal ="numTotal"
    :numCorrect = 'numCorrect'
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col class="quest-col">
          <QuestionBox
            v-if="questions.length"
           :currentQuestion="questions[questIndex]"
           :next="next"
           :increment = "increment"
           :questIndex= "questIndex"
           :questions= "questions"
           />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template> 

<script>
import Header from "./components/Header.vue"
import QuestionBox from "./components/QuestionBox.vue"

export default {
  name: 'App',
  components:{
    Header,
    QuestionBox
  },
  data(){
    return {
      questions : [],
      questIndex: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods:{
      next(){
        this.questIndex++
      },
      increment(isCorrect){
        if (isCorrect){
          this.numCorrect++
        }
        this.numTotal++
      }    
  },
  mounted: function() {
    let myHeaders = new Headers();
    myHeaders.append('Content-Type','text/plain; charset=UTF-8');
    fetch ("https://opentdb.com/api.php?amount=10&category=18&type=multiple" , {method:'get'})
    .then((response)=>{
      return response.json() })
    .then((jsonData)=>{
     this.questions =  jsonData.results })
  }

 }

</script>

<style>
*{
  box-sizing: border-box;
}
html {
    background-color:#2a75ff;
}
body{
  display: flex;
  align-items: center;
  justify-content: center;
  background: #2a75ff;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color:#2a75ff;
  height: 100%;
  
}
.quest-col{
margin-top:10px; 
}
.nav-item a {
  color:white;
}
#app > div:nth-child(2) > ul {
  border-bottom: none;
}
.list-group-item{
  border:none;
  border-bottom: 5px solid rgb(123 99 99 / 13%) ;
}
hr{
  width: 50%;
}
</style>


