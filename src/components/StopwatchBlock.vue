<template>
    <div :class="['stopwatch', {'stopwatch_active': isRunning}]">
        <div class="stopwatch__time">{{ formatTime }}</div>

        <div class="stopwatch__controls">
            <button
                v-if="!isRunning"
                @click="startStopwatch"
                class="stopwatch__start-stop">
                <img src="@/assets/icons/start.svg" alt="start">
            </button>

            <button
                v-else
                @click="stopStopwatch"
                class="stopwatch__start-stop">
                <img src="@/assets/icons/stop.svg" alt="stop">
            </button>

            <button
                @click="resetStopwatch"
                class="stopwatch__reset">
                <img
                    v-if="isRunning"
                    src="@/assets/icons/reset-active.svg"
                    alt="reset">

                <img
                    v-else
                    src="@/assets/icons/reset.svg"
                    alt="reset">
            </button>
        </div>
    </div>
</template>

<script setup>
import {computed, ref} from 'vue'

const props = defineProps({
    id: {
        type: String,
        required: true
    },
    time: {
        type: Number,
        required: true
    },
    isRunning: {
        type: Boolean,
        required: true
    },
    elapsedTime: {
        type: Number,
        required: true
    }
})

const time = ref(props.time)
const isRunning = ref(props.isRunning)
const intervalId = ref(null)
const startTime = ref(null)
const elapsedTime = ref(props.elapsedTime)

const startStopwatch = () => {
    if (!isRunning.value) {
        startTime.value = Date.now() - elapsedTime.value
        intervalId.value = setInterval(() => {
            time.value = Date.now() - startTime.value
        }, 10)
        isRunning.value = true
    }
}

const stopStopwatch = () => {
    if (isRunning.value) {
        clearInterval(intervalId.value)
        elapsedTime.value = Date.now() - startTime.value
        isRunning.value = false
    }
}

const resetStopwatch = () => {
    time.value = 0
    elapsedTime.value = 0
    isRunning.value = false
    clearInterval(intervalId.value)
}

const formatTime = computed(() => {
    const hours = Math.floor((time.value / 60000) / 60)
    const minutes = Math.floor(time.value / 60000)
    const seconds = Math.floor((time.value - minutes * 60000) / 1000)

    return `${hours.toString()}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})
</script>

<style scoped lang="scss">
.stopwatch {
    background-color: $grey;
    color: $light-grey;
    transition: .3s;

    &_active {
        color: #fff;
    }

    &__time {
        width: 100%;
        height: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 22px;
        border-bottom: 1px solid $light-grey;
    }

    &__controls {
        width: 100%;
        height: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    &__start-stop, &__reset {
        background-color: transparent;
    }

    &__reset {
        margin-left: 48px;
    }
}
</style>
