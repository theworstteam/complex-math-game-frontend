<template>
  <div class= "quetion-container">
      <Timer class="timer" 
            :timer="currentTimer"
            :state="timerState"
        />
      <QuestionBox
        v-if="questions.length"
        v-bind:currentQuestion="questions[index]"
        v-bind:tableNumber="tableNumber"
        v-bind:next="next"
        v-bind:increment="increment"
      />
      <b-modal ref="my-modal" hide-footer centered title="You have run out of time!">
           <template slot="modal-header" class="d-block center;"> 
              <h4>Correct : {{numCorrect}}</h4>
              <h4>Incorrect : {{numIncorrect}}</h4>
              <h4>Total : {{numTotal}}</h4><br>
          </template>
          <template>
            <div
              v-for=" (question,index) in resultQuestions"
              v-bind:key="index"
              class="center"
             >
            Question {{index+1}}: ( {{question.question}} ) 
            Time: {{laps[index].seconds}} second
            </div>
        </template>
          <b-button class="mt-3" variant="info" size="lg" block @click="goHome">Go back</b-button>
        </b-modal>
  </div>
</template>
<script>
import QuestionBox from "../components/QuestionBox"
import Timer from "../components/Timer";
import APIService from "../APIService";

export default {
    name: "Game",
    components: {
      QuestionBox,
      Timer
    },
    // props pass by the parent
    props: [
      "tableNumber",
    ],
    //attributes of the class
    data(){
      return{
        questions: [],
        resultQuestions: [],
        index: 0,
        numCorrect: 0,
        numIncorrect: 0,
        numTotal: 0,
        timerState: "stopped",
        currentTimer: 62,
        ticker: undefined,
        laps: [],
        countTimer: "",
        highScore: "",
      }
    },
    methods: {
      // change to the next question
      next() {
        this.index++
      },
      // counter method
      increment(isCorrect) {
        if(isCorrect){
          this.numCorrect++;
        }
        else{
          this.numIncorrect++;
        }
        this.numTotal++;
        this.lap();
        this.countTimer = 0;
        this.resultQuestions[this.index] = this.questions[this.index]
      },
       // start the timer
      start() {
        if (this.timerState !== "running") {
          this.tick();
          this.timerState = "running";
        }
      },
      // mark the current time
      lap() {
        this.laps.push({
          seconds: this.countTimer
        })
      },
      // stop the timer
      stop() {
        window.clearInterval(this.ticker);
        this.currentTimer = 60;
        this.timerState = "stopped";
      },
      // count down till timer stops
      tick() {
          this.ticker = setInterval(() => {
          this.currentTimer--;
          this.countTimer++;
            if(this.currentTimer === 0){
              this.stop()
              this.toggleModal()
        }
        }, 1000);
      }, 
      // Country roads take me home.. to the place I belong
      goHome() {
        this.$router.push('/') 
      },

      // update the high score if possible
      toggleModal() {
        this.$refs['my-modal'].toggle('#toggle-btn')
        const preHighScore = JSON.parse(JSON.stringify(this.highScore))[0].highscore
        if(this.numCorrect > preHighScore){
          APIService.postHighScore(this.tableNumber,this.numCorrect)
        }
      }
      },

      // get the api at the time page starts
      async created() {
        this.start();
        try {
          if (this.tableNumber == 13) {
            this.questions = await APIService.getAdvanceMultiplication();
          }else{
            this.questions = await APIService.getMultiplication(this.tableNumber);
          }
          this.highScore = await APIService.getHighScore(this.tableNumber);
        } catch (err) {
          this.err = err.message;
        }
      },

}
</script>

<style scoped>
  .timer{
        background: lightgray;
        text-align: center;
        color: gray;
        padding: 15px;
    }
    .center { 
      border: 2px solid lightgray;
      padding: 5px;
      margin: 10px
  }

</style>




