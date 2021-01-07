<template>
  <div>
    <h3>Pomodoro Timer</h3>
    <div id="options">
      <button @click="pauseTimer();"> Pause/ Resume </button>
      <button @click="changeDuration(pomodoroTime); currentPhase = 'work!';">Reset Timer </button>
      <button @click="changeDuration(shortBreak); currentPhase = 'take a short break!';"> Short Break </button>
      <button @click="changeDuration(longBreak); currentPhase = 'take a long break!';"> Long Break </button>
    </div>
    <div id="clock">
        <h1>{{displayTime}}</h1>
        <br>
        <h2>
          <div v-if="paused == false" style="display: inline"> {{currentPhasePreText}}</div>
          <div id="directions"> {{currentPhase}} </div>
        </h2>
    </div>
    <div id="options">
      <button @click="toggleSettings"> Settings </button>
    </div>
    <settings @changeTime="changeTimeMethod" :longBreak="longBreak" :shortBreak="shortBreak" :pomodoroTime="pomodoroTime" v-if="showSettings"/>
  </div>
</template>

<script>
var moment = require("moment");
import settings from './settings'
export default {
  name: "timer",
  data() {
    return {
      displayTime: null,
      endTime: null,
      timeLeft: null,
      longBreak: [0,10,1],
      shortBreak: [0,5,1],
      pomodoroTime: [0,25,1],
      test: null,
      paused: false,
      showSettings: false,
      currentPhasePreText: "It's time to: ",
      currentPhase: 'Work'
    };
  },
  components:{
    settings
  },
  methods: {
    changeTimeMethod(val){  

      this.longBreak = val.longTime;
      this.shortBreak = val.shortTime;
      this.pomodoroTime = val.defaultTime;
      this.changeDuration(this.pomodoroTime);
      },
    toggleSettings(){
      this.showSettings = !this.showSettings;
    },
    changeDuration(val){
        this.paused = false
        this.endTime = moment().add(val[2], "seconds").add(val[0], "hours").add(val[1], "minutes");
        this.timeLeft = moment.duration(this.endTime.diff(moment()));
    
    },
    pauseTimer(){
        if (this.paused == true) { 
          this.currentPhase = 'work!';
        }
        else{
          this.currentPhase = 'paused';
        }
        this.paused = !this.paused;
        
    },
    timer(){
      this.interval = window.setInterval(() => {
        if(this.paused === false){ 
            this.timerFormat()        
        }
        else {
          this.endTime.add(1, "seconds");
        }
      } 
      , 1000);
    },
    timerFormat(){
      this.timeLeft = moment.duration(this.endTime.diff(moment()));
      // placing time into individual variables
      let seconds= this.timeLeft.seconds()
      let minutes = this.timeLeft.minutes()
      let hours  = this.timeLeft.hours()
      if (seconds < 0){ seconds = 0 }
      if (minutes < 0){ minutes = 0}
      if (hours < 0){ hours = 0}
      // concatenating 0 to the beginning of the time if it is less than 2 digits
      if (hours.toString().length == 1 && hours >= 0 ){ hours = '0' + hours }
      if (minutes.toString().length === 1 && minutes >= 0){ minutes = '0' + minutes }
      if (seconds.toString().length == 1 && seconds >= 0){ seconds = '0' + seconds }
      // setting time variable 
      this.displayTime = hours + ":" + minutes + ":" + seconds;
    }
  },
  mounted() {
    this.timer();
    // window.setInterval(() => this.timer(), 1000);
  },
  created() {
    // this.endTime = moment().add(25, "minutes");
    this.changeDuration([0,25,2])
    // this.test = moment.duration(25,'minutes');
    // this.timeLeft = moment.duration(this.endTime - this.startTime, 'milliseconds')
  }
};
</script>
<style lang="scss">
$background : rgb(36, 36, 36);
$buttons : rgb(211, 194, 43);
$buttons-text: rgb(36, 36, 36);
$light: whitesmoke;
body {
  background-color: $background;
}
#clock {
  /* height: 500px; */
  /* width: 500px; */
  /* box-shadow: 0px 10px 40px 30px rgb217, 255, 0); */
  
  width: 100%;
  /* display: inline-block; */
  position: relative;
}
#clock > h1 {
  color: black;
  border-radius: 5px;
  background-color: #c2f8cb;
  margin: auto;
  padding: 20px;
  display: inline-block;
  font-family: monospace;
  font-size: 64px;
}

#options {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-evenly;
  width: 30%;
  margin: auto;
  /* width: 500px; */
  /* top: 60%; */
  /* -ms-transform: translateY(-50%); */
  /* transform: translateY(-50%); */
  @media screen and (max-width: 500px){
    
  }

}
#options > button {
  // border: 3px solid #5603ad;
  border: none;
  // border-radius: 5px;
  font-family: monospace;
  font-size: 23px;
  margin: 20px;
  background-color: $buttons;
  /* outline-style: none; */
  color: $buttons-text;
  font-weight: bolder;
  /*  box-shadow: 2px 2px 2px rgb(226, 220, 220); */
  text-decoration: none;
  /* border: none; */
   &:hover {
     background-color: darken($buttons, 15%);
     color: lighten($buttons-text, 75%);
   }
}
h3 {
  color: rgb(255, 255, 255);
  font-family: monospace;
  font-size: 45px;
  /* text-shadow: 2px 2px 10px rgb(217, 255, 0); */
}
h2 {
  color: $light;
}
#directions {
  text-decoration: underline;
  font-weight: 600px;
}
</style>