<template>
  <view class="container">
    <view v-if="!workoutActive">
      <button
        :on-press="startWorkout"
        title="Start 10 min Workout"
        color="blue"
        accessibility-label="Learn more about this purple button"
      />
    </view>
    <view class="timer-container" v-if="!workoutCompleted && workoutActive">
      <!-- <text class="text-color-primary">Workout Timer</text> -->
      <text class="text-color-primary timer-step">{{tm.name}}</text>
      <text class="text-color-primary timer-value" :class="{'is-rest': isRest}">{{tm.value}}</text>
    </view>

    <view v-if="workoutCompleted">
      <text class="text-color-primary">Workout Completed Successfull!</text>
      <button
        :on-press="startWorkout"
        title="Run again"
        color="green"
        accessibility-label="Learn more about this purple button"
      />
    </view>
  </view>
</template>
<script>
export default {
  methods: {
    startWorkout: function() {
      //alert("Hello");
      this.workoutActive = true;
      this.runTimer(this.timer, this.getNext).then(this.workoutCompleted);
      //this.runTimer();
    },
    play() {
      //const audio = new Audio("./assets/audio/beep.mp3");
      //audio.play();
    },
    workoutCompleted() {
      // document.getElementById(
      //   "safeTimerDisplay"
      // ).innerHTML = `Workout completed!!! See you next time!`;
      // console.log("completed successfull");
      this.workoutCompleted = true;
    },
    timer(a) {
      const that = this;
      return new Promise(function(res) {
        //alert("v");
        const { name, duration: sec } = a;
        that.tm.value = sec;
        that.tm.name = name;
        const timer = setInterval(() => {
          // document.getElementById(
          //   "safeTimerDisplay"
          // ).innerHTML = `${name}: осталось ${s}`;
          that.tm.value--;
          if (that.tm.value < 0) {
            clearInterval(timer);
            that.play();
            res();
          }
        }, 1000);
      });
    },
    runTimer(action, getNext) {
      const that = this;
      return new Promise(res => {
        (function runItem() {
          const item = getNext();
          that.timerItem = item;
          if (item == null) {
            //that.timerItem = { name: "item = null" };
            that.workoutCompleted = true;
            res();
          } else {
            //that.timerItem = { name: "item = NOT null" };
            that.timer(item).then(runItem);
          }
        })();
      });
      //this.timerItem = { name: "ex1", value: 2 };
      //alert("timer ran");
    }
  },
  data() {
    return {
      timerData: [
        { name: "01. High knees", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "02. Low plank to high plank", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "03. Half burpee", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "04. Toe touches", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "05. Squat jump", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "06. RIGHT side plank", duration: 20 },
        { name: "Rest", duration: 3 },
        { name: "07. LEFT side plank", duration: 20 },
        { name: "Rest", duration: 15 },
        { name: "08. Alternate lunges", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "09. Jumping jack", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "10. Hip thrust", duration: 45 },
        { name: "Rest", duration: 15 },
        { name: "11. Butt kicks", duration: 45 }
      ],
      workoutCompleted: false,
      workoutActive: false,
      tm: {
        name: "",
        value: 0
      },
      timerItem: {},
      getNext: null,
      isRest: true
    };
  },
  mounted() {
    this.getNext = this.timerData.shift.bind(this.timerData);
    // this.$nextTick(function() {
    // });
  }
};
</script>
<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.text-color-primary {
  color: blue;
}
.timer-container {
  flex: 1;
  background-color: white;
  align-items: center;
  justify-content: space-between;
}
.timer-value {
  font-size: 180;
  font-weight: bold;
  color: red;
}
.timer-value.is-rest {
  color: green;
}
.timer-step {
  margin-top: 50px;
  font-size: 22px;
}
</style>
