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
            <button @click="reset" class="btn btn-danger">Terminar</button>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import bip from '../static/sound.mp3'
import go from '../static/go.mp3'

export default {
  name: "IndexPage",
  data() {
    return {
      isRunning: false,
      workoutSound: null,
      restSound: null,
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
      workingout: true,
      resting: false,
    }
  },
  mounted() {
    this.workoutSound = new Audio(go)
    this.restSound = new Audio(bip)
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
    setTime({ workout, rest, series }) {
      this.workoutTime = (workout.minutes * 60 + workout.seconds)
      this.restTime = (rest.minutes * 60 + rest.seconds)
      this.series = series
      this.startWorkout = true
    },
    runWorkout(workout, rest) {
      if (this.workoutTime > 0) {
        this.workoutTime--
        return
      }
      this.workingout = false
      this.resting = true
      this.restSound.play()
    },
    runRest(workout, rest) {
      if (this.restTime > 0) {
        this.restTime--
        return
      }
      this.workingout = true
      this.resting = false
      if (this.series > 1) {
        this.workoutSound.play()
      }
      this.restTime = rest
      this.workoutTime = workout
      this.series--
    },

    start() {
      this.isRunning = true
      const workout = this.workoutTime
      const rest = this.restTime
      this.workoutSound.play()
      this.timer = setInterval(() => {
        if (this.series === 0) {
          console.log('fin')
          this.reset()
          return
        }
        if (this.workingout) {
          console.log('entrenando')
          this.runWorkout(workout, rest)
          return
        }
        if (this.resting) {
          console.log('descansando')
          this.runRest(workout, rest)
          return
        }
      }, 1000)
    },

    stop() {
      this.isRunning = false
      clearInterval(this.timer)
      this.timer = null
    },
    reset() {
      clearInterval(this.timer)
      this.workingout, this.resting = false
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
  }
}
</script>

<style>
.timer {
  font-size: 2em;
  margin: 20px;
}
</style>