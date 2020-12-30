<template>
  <div id="app">
    <Header 
     :numCorrect="numCorrect"
     :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-container class="bv-example-row">
          <b-row>
            <b-col sm="6" offset="3">
              <QuestionBox
                v-if="questions.length"
                :CurrentQquestion="questions[index]"
                :NextQuestion="NextQuestion"
                :increment="increment"
              />
            </b-col>
          </b-row>
        </b-container>
      </b-row>
    </b-container>
  </div>
</template>

<script>
 import response from './jsondb.json'
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";


export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect:0,
      numTotal:0
    };
  },
  methods: {
    NextQuestion() {
      this.index++;
    },
    increment(isCorrect){
      console.log("isCorrect", isCorrect)
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    
        this.questions = response.results;
      }
  
};
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
