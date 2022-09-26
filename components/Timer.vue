<template>
    <div>
        <div class="timer">
            <p>Serie: <span class="value">{{time.series}}</span></p>
        </div>
        <div class="timer">
            <p> Ejercicio: <span class="value">{{ time.workout | prettify }} </span></p>
        </div>
        <div class="timer">
            <p> Descanso: <span class="value">{{ time.rest | prettify }} </span> </p>
        </div>
    </div>
</template>

<script>
export default {
    props: ['time'],
    filters: {
        prettify: function (value) {
            let data = value.split(':')
            let minutes = data[0]
            let secondes = data[1]
            if (minutes < 10) {
                minutes = "0" + minutes
            }
            if (secondes < 10) {
                secondes = "0" + secondes
            }
            return minutes + ":" + secondes
        }
    }

}
</script>

<style lang="scss" scoped>
.value {
    font-weight: bold;
}
</style>