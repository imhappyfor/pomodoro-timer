<template>
  <div id="container">
      <button @click="changeDefault = !changeDefault">Change Default Time</button>
      <button @click="changeShort = !changeShort">Change Short Break</button>
      <button @click="changeLong = !changeLong">Change Long Break</button>
      <br>
      <div v-if="changeDefault">
        <p>Please select the default amount of time you would like for the <span class="define-text"> pomodoro </span> </p>
                <h4>Hours:  <input type="number" max="11" min="0" v-model="defaultTime[0]"> </h4>
                <h4>Minutes:  <input type="number" max="59" min="0" v-model="defaultTime[1]"> </h4>
                <h4>Seconds:  <input type="number" max="59" min="0" v-model="defaultTime[2]"> </h4>
      </div>
        <br>
        <div v-if="changeShort">
            <p>Please select the default amount of time you would like for the <span class="define-text"> short </span> break</p>
                <h4>Hours:  <input type="number" max="11" min="0" v-model="shortTime[0]"> </h4>
                <h4>Minutes:  <input type="number" max="59" min="0" v-model="shortTime[1]"> </h4>
                <h4>Seconds:  <input type="number" max="59" min="0" v-model="shortTime[2]"> </h4>
        </div>
        <br>
        <div v-if="changeLong">
            <p>Please select the default amount of time you would like for the <span class="define-text">  long </span> break</p>
                <h4>Hours:  <input type="number" max="11" min="0" v-model="longTime[0]"> </h4>
                <h4>Minutes:  <input type="number" max="59" min="0" v-model="longTime[1]"> </h4>
                <h4>Seconds:  <input type="number" max="59" min="0" v-model="longTime[2]"> </h4>
        </div>
        <button @click="save">Save</button> 
        <p>NOTE: these settings will reset on page reloads.</p>
  </div>
</template>

<script>
export default {
    data(){
        return{
            changeDefault: true,
            changeShort: false,
            changeLong: false,
            defaultTime: [0,25,0],
            longTime: [0,10,0],
            shortTime: [0,5,0]
        }
    },

    methods: {
        save(){
            this.validate(0);
            this.validate(1);
            this.validate(2);
            this.$emit('changeTime', {'defaultTime':this.defaultTime,'longTime' : this.longTime,'shortTime': this.shortTime});
        },
        validate(digit){
            if(this.defaultTime[digit] < 0) { 
                alert('Digits less than 0 are not allowed, silly!')
                this.defaultTime[digit] = 0
            }
            if(this.longTime[digit] < 0) { 
                alert('Digits less than 0 are not allowed, silly!')
                this.longTime[digit] = 0
            }
            if(this.shortTime[digit] < 0) { 
                alert('Digits less than 0 are not allowed, silly!')
                this.shortTime[digit] = 0
            }
        },
    }
}
</script>

<style lang="scss">
$buttons : rgb(211, 194, 43);
$buttons-text: rgb(36, 36, 36);
p  {
    font-weight: 600;
    color: rgb(211,194,43);
}
#container {
    border: 3px solid #5603ad;
    color: rgb(211,194,43);
    width: 50%;
    margin-left: 25%;
}
.define-text {
    font-size: 20px;
    color: rgb(79, 195, 206);
    }
button {
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
</style>