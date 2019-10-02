<template>
  
  <div>
    
    <b-jumbotron class="mt-3">      
     
      <p v-html="currentQuestion.question"> </p>

      <hr/>

      <b-list-group>

      <b-list-group-item 
        v-for="(answer,index) in answers"
        :key="index"
        @click.prevent = "selectAnswer(index)"
        :class = "[selectedIndex === index ? 'selected' : '']"
        v-html="answer"
      >
      </b-list-group-item>

      </b-list-group>

      <div style="height:24px"></div>


      <b-card bg-variant="success" text-variant="white" class="text-center mb-4" v-if="info_correct">
        <div style="display:flex;justify-content:space-between">
        <b-card-text>
          You are correct!
          
        </b-card-text>
        <b-button @click="proceed" class="ml-2" variant="light" href="#" :disabled="selectedIndex == null">Continue</b-button>
        </div>
      </b-card>

      <b-card bg-variant="danger" text-variant="white" class="text-center mb-4" v-if="info_wrong">
        <div style="display:flex;justify-content:space-between">
        <p>
        
          You are wrong!
          The answer is {{current_correct_answer}}
        </p>
        <div>
        <b-button @click="proceed" class="ml-2" variant="light" href="#" :disabled="selectedIndex == null">Continue</b-button>
        </div>
        </div>

      </b-card>

      <div v-if="show_buttons" style="display:flex;justify-content:space-between;">
        <b-button variant="outline" href="#">Skip</b-button>      
        <b-button @click="checkAnswer" class="ml-2" variant="primary" href="#" :disabled="selectedIndex == null">Submit</b-button>
      </div>

    </b-jumbotron>

  </div>

</template>


<script>

import _ from 'lodash'

export default {
  props:{
    currentQuestion: Object,
    next: Function,
  },
  data: function(){
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      info_correct: false,
      info_wrong: false,
      show_buttons: true,
      current_correct_answer: null,
      
    }
  },
  mounted:function(){
    console.log('currentQuestion',currentQuestion);
    // this.shuffleAnswers();
  },  
  computed:{
    answers(){
      let answers = [ ...this.currentQuestion.incorrect_answers ];     
      answers.push(this.currentQuestion.correct_answer); 

      this.current_correct_answer = this.currentQuestion.correct_answer;

      return _.shuffle(answers);

    }
  },
  methods: {
    selectAnswer(index){

      console.log('hello: ' + index); 
      this.selectedIndex = index;

    },
    shuffleAnswers(){
      
      let answers = [ ...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);      

    },
    proceed(){
      
      this.info_correct = false;
      this.info_wrong = false;
      this.show_buttons = true;
      this.selectedIndex = null;

      this.next();


    },    
    checkAnswer(){
      
      console.log('check answer');     

      console.log(this.answers[this.selectedIndex]);

      let my_answer = this.answers[this.selectedIndex];

      if (my_answer === this.current_correct_answer){
        this.info_correct = true;
        this.info_wrong = false;
      }else{
        this.info_wrong = true;
        this.info_correct = false;
      }

      this.show_buttons = false; 


    },
    resetPanel(){
      this.info_correct = false;
      this.info_wrong = false;
      this.show_buttons = true;
    },
  },
  watch: {

  },


}
</script>

<style scoped>

.list-group{
  margin: 12px;
}

.list-group-item:hover{
  /* background: #eee; */
  cursor: pointer;
}

.selected {
  background-color: rgb(163, 181, 198);
}

.correct {
  background-color: aquamarine;

}

.incorrect {
  background-color: rgb(159, 0, 0);
}
 
</style>