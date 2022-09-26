<template>
    <div>
        <form>
            <div class="my-2">
                <h2>Series</h2>
                <label for="series">
                    <input type="number" v-model="series" name="series" id="series" min="0" class="form-control">
                </label>
            </div>
            <div class="my-2">
                <h2>Ejercicio</h2>
                <label for="min">
                    <input type="number" v-model="workout.minutes" name="time_m" id="min" min="0" max="59"
                        class="form-control">
                </label>
                <span class="double-dot">:</span>
                <label for="sec">
                    <input type="number" v-model="workout.seconds" name="time_s" id="sec" max="59" min="0"
                        class="form-control">
                </label>
            </div>
            <div class="my-2">
                <h2>Descanso</h2>
                <label for="min">
                    <input type="number" v-model="rest.minutes" name="time_m" id="min" min="0" max="59"
                        class="form-control">
                </label>
                <span class="double-dot">:</span>
                <label for="sec">
                    <input type="number" v-model="rest.seconds" name="time_s" id="sec" max="59" min="0"
                        class="form-control">
                </label>
            </div>
            <div>
                <button type="button" @click="sendTime" class="btn btn-primary mt-4">Iniciar 🏋</button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            workout: {
                minutes: 0,
                seconds: 0
            },
            rest: {
                minutes: 0,
                seconds: 0
            },
            series: 0
        }
    },
    methods: {
        sendTime() {
            this.$emit('set-time', { workout: this.workout, rest: this.rest, series: this.series })
        }
    }
}
</script>

<style lang="scss" scoped>
.double-dot {
    font-weight: bold;
    font-size: 22px;
}
</style>