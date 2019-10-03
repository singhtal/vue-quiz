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
                :class="[selectedIndex === index ? 'correct' : '']"
                >{{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" href="#">Submit</b-button>
            <b-button @click="next"
            variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    export default {
        props: {
            currentQuestion: Object,
            next: Function
        },
        data(){
            return{
                selectedIndex: null
            }
        },
        computed: {
            answers(){
                if(this.currentQuestion){
                    let answers = [...this.currentQuestion.incorrect_answers];
                    answers.push(this.currentQuestion.correct_answer)
                } else {
                    let answers = [];
                }

                return answers;

            }
        },
        methods: {
            selectAnswer(index){
                this.selectedIndex = index;
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
    background-color: blue;
}
.correct{
    background-color: green;
}
.incorrect{
    background-color: red;
}
</style>