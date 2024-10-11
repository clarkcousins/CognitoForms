<script setup>
import questions  from "./questions";
</script>

<template>
  <div v-if="count < this.q.length">
    <div class="container">
      <p class = "questionCounterStyle"> Question #{{ count + 1}} of {{ q.length }}</p>
      <h2 class = "questionStyle">{{ q[count]['text'] }}</h2>
        <ul class = "answersList" v-for="(answer, index) in q[count]['answers']" :key="index">
          <div v-if="!displayPopUp">
            <button 
              :class= "filledAnswer == index ? 'filled' : 'quizButton' " 
              @click="quizClicked(index)"
            > {{ answer }} </button>
          </div>
        </ul>
    </div>
  <div v-if="displayPopUp"> 
    <button class="popUp"> Select an answer! </button>
  </div>
  <button class="nextButton" @click=next(count)>&#8594</button>
  </div>
  <div v-else>
    <h2 class = "questionStyle">Quiz Recap</h2> 
    <ul v-for="(_, index) in q" :key="index">
      <h3>Question #{{index + 1}}: {{ q[index]['text'] }} </h3>
      <span>Your Answer: {{ q[index]['answers'][answers[index]] }}</span>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  
  data() {
    return {
      count: 0,
      q: questions,
      filledAnswer: null,
      displayPopUp: false,
      answers: []
    };
  },

  methods: {
    next(count) {
      if (count < this.q.length & this.filledAnswer != null) {
        this.answers.push(this.filledAnswer);
        this.count++;
        this.filledAnswer = null;
        this.displayPopUp = false;
      } else {
        this.displayPopUp = true;
        setTimeout(() => {
          this.displayPopUp = false;
        }, 1500);
      }
    },
    quizClicked(index) {
      this.filledAnswer = index;
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.answersList { 
  list-style: none;
  padding: 0;
}
.questionStyle {
  justify-content: center;
  display: flex;
  align-items: center;
  font-size: 1.5em;
  font-family: Arial, Helvetica, sans-serif;
}
.questionCounterStyle {
  justify-content: center;
  display: flex;
  align-items: center;
  font-size: 1.1em;
  font-family: Arial, Helvetica, sans-serif;
}
.nextButton {
  height: 40px; 
  width: 80px;
  border-radius: 15%;
  padding: 0px;
  line-height: 25px;
  font-size: 1.25em;
  position: fixed;
  bottom: 20px;
  right: 20px;
}
.quizButton {
  height: 50px;
  width: 350px;
  border-radius: 2%;
  transition: background-color 0.2s ease;
  background-color: rgba(0, 128, 0, 0.5);
  color: black;
  border-radius: 15%;
  font-size: 1.35em;
  font-family: Arial, Helvetica, sans-serif;
  z-index: 0;
  position: relative;
}
.filled {
  background-color: rgba(0, 128, 0, 1);
  color: white;
  height: 50px;
  width: 350px;
  border-radius: 15%;
  font-size: 1.35em;
  font-family: Arial, Helvetica, sans-serif;
  z-index: 0;
  position: relative;
}
.popUp {
  border-color: red;
  background-color: pink;
  color: red;
  border-width: 2px;
  height: 150px;
  width: 450px;
  border-radius: 15%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.5em;
  font-family: Arial, Helvetica, sans-serif;
  z-index: 1;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}

</style>
