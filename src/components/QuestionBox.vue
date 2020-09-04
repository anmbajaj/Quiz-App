<template>
    <div class="question_box_container">
        <b-jumbotron>
            <template v-slot:lead>
                {{currentQuestion.question}}
            </template>
            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                    v-for="(answer,index) in answers" 
                    :key="index"
                    @click="selectAnswer(index)"
                    :class="[answerClass(index)]">
                        {{answer}}
                        </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" 
                @click="submitAnswer"  
                :disabled="selectedIndex === null || answered === true ? true: false " 
            >
                Submit
            </b-button>
            <b-button @click="next"  variant="success" 
                :disabled="selectedIndex != null && answered == true ? false : true"
            >Next -></b-button>
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
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            answered: false
        }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex=index
        },
        submitAnswer(){
            let isCorrect = false;

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true;
            }
            this.answered=true
            this.increment(isCorrect);
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass(index){
            let answerClass = ''
            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected-answer'
            } else if(this.answered && this.correctIndex === index) {
                answerClass = 'correct-answer';
            } else if(this.answered && this.selectedIndex === index && this.correctIndex != index) {
                answerClass = 'incorrect-answer';
            }
            return answerClass;
        }
    },
    computed: {
        answers(){
            this.shuffleAnswers()
            return this.shuffledAnswers
        }
    },
    watch:{
        currentQuestion() {
            this.selectedIndex = null;
            this.correctIndex = null;
            this.answered = false;
        }
        
        // (){
        //     this.selectedIndex=null
        //     this.shuffleAnswers();
        // }
    },
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
}

.list-group-item:hover{
    background-color: #EEE;
    cursor: pointer;
}

.btn{
    margin: 0 5px;
}

.selected-answer{
    background-color: lightblue;
}

.correct-answer{
    background-color: lightgreen;
}

.incorrect-answer{
    background-color: #FF6347;
}

</style>