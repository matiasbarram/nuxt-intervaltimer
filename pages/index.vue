<template>
  <div>
    <div id="main">
      <div class="container">
        <h1 class="py-5">🏄🏇🏊🏋🚴🏂</h1>
        <TimerSetup v-if="!startWorkout" @set-time="setTime"></TimerSetup>
        <div v-else>
          <Timer :time="prettyTime"></Timer>
          <div>
            <button v-if="!isRunning" @click="start" class="btn btn-success">Inicio</button>
            <button v-if="isRunning" @click="stop" class="btn btn-warning">Pausa</button>
            <button @click="reset" class="btn btn-danger">Reinicio</button>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import bip from '../static/sound.mp3'

export default {
  name: "IndexPage",
  data() {
    return {
      isRunning: false,
      sound: null,
      startWorkout: false,
      workout: {
        minutes: 0,
        seconds: 0
      },
      rest: {
        minutes: 0,
        seconds: 0
      },
      series: 0,
      workoutTime: 0,
      restTime: 0,
      timer: null,
    }
  },
  mounted() {
    this.sound = new Audio(bip)
  },
  computed: {
    prettyTime() {
      let workoutTime = this.parseTime(this.workoutTime)
      let restTime = this.parseTime(this.restTime)
      return { workout: workoutTime, rest: restTime, series: this.series }
    }
  },
  methods: {
    parseTime(inputTime) {
      let time = inputTime / 60
      let minutes = parseInt(time)
      let seconds = Math.round((time - minutes) * 60)
      return minutes + ":" + seconds
    },

    start() {
      this.isRunning = true
      const workout = this.workoutTime
      const rest = this.restTime
      this.timer = setInterval(() => {
        // Time for workout
        if (this.workoutTime > 0) {
          if (this.workoutTime === workout && this.restTime === rest) {
            this.sound.play()
          }
          this.workoutTime--
        }
        else {
          // Time for rest
          if (this.workoutTime <= 0 && this.restTime > 0) {
            if (this.workoutTime === 0 && this.restTime === rest) {
              this.sound.play()
            }
            this.restTime--
          }
          // New serie
          else {
            this.series--
            this.workoutTime = workout
            this.restTime = rest
          }
        }
      }, 1000)
    },
    stop() {
      this.isRunning = false
      clearInterval(this.timer)
      this.timer = null
    },
    reset() {
      this.stop()
      this.workoutTime, this.restTime, this.series = 0
      this.workout = {
        minutes: 0,
        seconds: 0
      }
      this.rest = {
        minutes: 0,
        seconds: 0
      }
      this.startWorkout = false
    },
    setTime({ workout, rest, series }) {
      this.workoutTime = (workout.minutes * 60 + workout.seconds)
      this.restTime = (rest.minutes * 60 + rest.seconds)
      this.series = series
      this.startWorkout = true
    }
  }
}
</script>

<style>
.timer {
  font-size: 2em;
  margin: 20px;
}
</style>