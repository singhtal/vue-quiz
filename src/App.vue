<template>
  <div id="app">
    <Header 
    :index="index"
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" >
        <QuestionBox 
        :v-if="questions.length"
        :currentQuestion="questions[index]"
        :index="index.toString()"
        :next="next"
        :increment="increment"
        />
        
        </b-col>
      </b-row>
    </b-container>

    
  </div>
</template>

<script>
import Header from './components/Header.vue';
import QuestionBox from './components/QuestionBox.vue';


export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
        questions: [],
        index: 0,
        numCorrect: 0,
        numTotal: 0
    }
  },
  methods: {
    next(){
      this.index++;
    },
    increment(isCorrect){
      console.log('inside inc method');

    if(isCorrect){
      this.numCorrect++;
    }
    this.numTotal++;
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10', {
      method: 'get',
      dataType: JSON
    })
    .then((response) => {
      //console.log('first response - ', JSON.stringify(response));
       return response.json();
    })
    .then((jsonData) => {
      //console.log('response sent - ',  jsonData.results)
      this.questions = jsonData.results;
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
