<template>
    <div class="text-left">
        <b-jumbotron>

            <template v-slot:lead>
            
            {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group v-for="(answer, index) in answers" :key="index">
                <b-list-group-item 
                @click="selectAnswer(index)"
                :class="[selectedIndex === index ? 'selected' : '']"
                >{{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button @click="submitAnswer" variant="primary" href="#">Submit</b-button>
            <b-button @click="next"
            variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>

import _ from 'lodash'
    export default {
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        data(){
            return{
                selectedIndex: null,
                shuffledAnswers: []
            }
        },
        computed: {
            answers(){
                let answers = [];
                if(this.currentQuestion){
                    answers = [...this.currentQuestion.incorrect_answers];
                    answers.push(this.currentQuestion.correct_answer)
                } else {
                    answers = [];
                }

                return answers;

            }
        },
        watch: {
               currentQuestion: {
                   immediate: true,
                   handler(){
                    this.selectedAnswer = [],
                    this.shuffleAnswers();      
                   }
               }

        },
        methods: {
            selectAnswer(index){
                console.log('selected index - '+index, ' correct index - unknown');
                this.selectedIndex = index;
            },
            submitAnswer(){
                console.log('submit fired');

                let isCorrect = false;

                if(this.selectedIndex == this.correctIndex){
                    isCorrect = true;
                }

                this.increment(isCorrect);

                // return isCorrect;
            },
            shuffleAnswers(){
                console.log('shuffling answers');
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer ];
                this.shuffledAnswers = _.shuffle(answers);

                return this.shuffledAnswers;
            }
        }
    }
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
}
.list-group-item:hover{
    background-color: #eee;
    cursor:pointer;
}
.btn{
    margin: 0 10px;
}
.selected{
    background-color: lightblue;
}
.correct{
    background-color: lightgreen;
}
.incorrect{
    background-color: red;
}
</style>