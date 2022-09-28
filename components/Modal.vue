<template>
    <div>
        <h2>{{title}} Terminado!</h2>
        <div>
            <Button class="btn btn-primary" @click="$emit('start')">Sigamos!</Button>
                <Button class="btn btn-secondary" @click="$emit('close')">Cancelar</Button>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['title'],

    }
</script>

<style lang="scss" scoped>

</style>
