<template>
  <div id="app">

    <Header :index = "index" />

    <b-container class="">
      <b-row class="d-flex justify-content-center">

        <b-col md="8">
          <QuestionBox 
            v-if="questions.length"
            :currentQuestion = "questions[index]"
            :next = "next"
          />
        </b-col>
      </b-row>      

    </b-container>

    
  </div>
</template>

<script>

import Header from './components/Header'
import QuestionBox from './components/QuestionBox'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
    }
  },
  methods:{
    next(){
      this.index++;
    },
     
  },
  mounted: function(){

    console.log('mounted');

    fetch('https://opentdb.com/api.php?amount=10&category=9&difficulty=easy&type=multiple', {
      method:'get',
    }).then((response)=>{
      return response.json();
    }).then((json)=>{
      this.questions = json.results;
      console.log('json.result ', json.results);
    })   

  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 5px;
}
</style>
