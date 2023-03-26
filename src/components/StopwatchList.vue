<template>
    <div class="stopwatch-list">
        <stopwatch-block
            v-for="stopwatch in stopwatches"
            :key="stopwatch.id"
            :id="stopwatch.id"
            :time="stopwatch.time"
            :is-running="stopwatch.isRunning"
            :elapsed-time="stopwatch.elapsedTime"/>

        <button
            @click="addTimer"
            class="stopwatch-list__add">
            <img src="@/assets/icons/plus.svg" alt="plus">
        </button>
    </div>
</template>

<script setup>
import {onMounted, ref} from 'vue'
import StopwatchBlock from '@/components/StopwatchBlock.vue'

const stopwatches = ref([])

const addTimer = () => {
    const id = Date.now().toString()
    const stopwatch = {id, time: 0, isRunning: false, elapsedTime: 0}
    stopwatches.value.push(stopwatch)
}

onMounted(() => addTimer())
</script>

<style scoped lang="scss">
.stopwatch-list {
    display: grid;
    grid-template-columns: 225px 225px 225px;
    grid-auto-rows: 120px;
    grid-column-gap: 50px;
    grid-row-gap: 45px;
    justify-content: center;

    @media (max-width: $tablet) {
        grid-template-columns: 225px 225px;
    }

    @media (max-width: $mobile) {
        grid-template-columns: 225px;
    }

    &__add {
        background-color: $grey;
    }
}
</style>
