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
                :class="answerClass(index)"
                >{{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button 
                @click="submitAnswer" 
                variant="primary" href="#"
                :disabled="selectedIndex === null || answered"
                >
                Submit
            </b-button>

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
                shuffledAnswers: [],
                correctIndex: null
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
                    console.log('inside watch handler');
                    this.selectedAnswer = [],
                    this.shuffleAnswers();
                    this.answered = false;      
                   }
               }

        },
        methods: {
            selectAnswer(index){
                this.selectedIndex = index;
            },
            submitAnswer(){
                console.log('submit fired');

                let isCorrect = false;

                if(this.selectedIndex == this.correctIndex){
                    isCorrect = true;
                }

                this.increment(isCorrect);
                this.answered = true;
                this.selectedIndex = null

                // return isCorrect;
            },
            shuffleAnswers(){
                console.log('shuffling answers');
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer ];
                this.shuffledAnswers = _.shuffle(answers);
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);

                return this.shuffledAnswers;
            },
            answerClass(index){

                let answerClass = '';

                if(!this.answered && 
                this.selectedIndex === index){
                    answerClass = 'selected';
                } else if(this.answered && 
                this.selectedIndex == this.correctIndex){
                    answerClass = 'correct';
                } else if(this.answered && 
                (this.selectedIndex == index) && 
                index != this.correctIndex){
                    answerClass = 'incorrect';
                }
                console.log(answerClass);

                return answerClass;
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