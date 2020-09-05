<template>
    <div class='quizbox'>
    <b-jumbotron header="BootstrapVue" lead="Bootstrap v4 Components for Vue.js 2">
    <template v-slot:header>Manga Quiz</template>

    <template v-slot:lead>
      {{ CurrentQuestion.question }}
    </template>
    <b-list-group>
        <b-list-group-item v-for="(answer,index) in answers" :key="index"
        @click.prevent="SelectAnswer(index)"
        :class="answerClass(index)"
        >
      {{ answer }}</b-list-group-item>
    </b-list-group>

    <hr class="my-4">

    <b-button variant="primary"
    @click="SubmitAnswer"
    :disabled="SelectedIndex===null || answered">
    Submit</b-button>
    <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default{
    props: {
        CurrentQuestion:Object,
        next:Function,
        increment:Function
    },
    data:function(){
        return{
            SelectedIndex: null,
            CorrectIndex:null,
            ShuffledAnswers:[],
            answered:false
        }
    },
    computed: {
        answers(){
            let answers=[...this.CurrentQuestion.incorrect_answers] 
            answers.push(this.CurrentQuestion.correct_answer)
            return answers
        }
    },
    watch:{
        CurrentQuestion:{
            immediate:true,
            handler(){
            this.SelectedIndex=null,
            this.answered=false
            this.ShuffleAnswers()
        }}
    },
    methods:{
        SelectAnswer(index){
            this.SelectedIndex=index
            console.log(index)
        },
        SubmitAnswer(){
            let iscorrect=false
            if(this.SelectedIndex===this.CorrectIndex){
                iscorrect=true
            }
            this.answered=true
            this.increment(iscorrect)
        },
        answerClass(index){
            let aclass=''
            if(this.SelectedIndex===index && !this.answered){
                aclass='selected'
            }else if(this.answered && this.CorrectIndex===index){
                aclass ='correct'
            }else if(this.answered && this.SelectedIndex === index && this.CorrectIndex!==index){
                aclass='incorrect'
            }
            return aclass
        },
        ShuffleAnswers(){
            let answers = [...this.CurrentQuestion.incorrect_answers,this.CurrentQuestion.correct_answer]
            this.ShuffledAnswers=_.shuffle(answers)
            this.CorrectIndex = this.ShuffledAnswers.indexOf(this.CurrentQuestion.correct_answer)
        }
    },
    mounted() {
        console.log(this.CurrentQuestion)
    }
}
</script>

<style scoped>
.list-group{
    margin-bottom:15px;
}
.list-group-item:hover{
    background-color:#EEE;
    cursor:pointer;
}
.btn{
    margin: 0 5px;
}
.selected{
    background-color:lightblue;
}
.correct{
    background-color:lightgreen;
}
.incorrect{
    background-color:#fc4e4e;
}
</style>