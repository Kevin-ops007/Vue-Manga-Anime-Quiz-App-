<template>
  <div id="app">
  <Header 
  :numCorrect="numCorrect"
  :numTotal="numTotal"
  />
  <b-container class="bv-example-row">
  <b-row sm="6" offset="3">
      <b-col><questionbox 
              v-if="questions.length"
              :CurrentQuestion="questions[index]"
              :next="next"
              :increment="increment"
              /></b-col>
    </b-row>
  </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import questionbox from './components/questionbox.vue'
//import Vue from 'vue'

export default {
  name: 'App',
  components: {
    Header,
    questionbox
  },
  data(){
    return{
      questions:[],
      index:0,
      numCorrect:0,
      numTotal:0
    }
  },
  methods:{
    next(){
      this.index++
    },
    increment(iscorrect){
      if(iscorrect)
        this.numCorrect++
      this.numTotal++  
    }
  },
  mounted:function(){
    fetch('https://opentdb.com/api.php?amount=10&category=31&difficulty=medium&type=multiple',{
      method:'get'
    })
    .then((response)=>{
      return response.json()
    })
    .then((jsonData)=>{
      this.questions= jsonData.results
    })
  }
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
