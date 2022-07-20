<template lang="pug" >
v-row.text-center#home
  //-v-col(cols="12")
    //-.w-50(background=" linear-gradient(to top, red 0%, red 1%, transparent 1%,transparent 100%)")
    //- v-img.w-30(src="https://github.com/alsoson/pomodoro/blob/master/src/assets/images/%E6%BB%BF.png?raw=true")
  v-col(cols="12")
    #square(background="white")
      h1 {{ timeText }}
      v-btn.mx-3(icon color="#55a6a1" v-if="status !== 1" @click="startTimer")
        v-icon mdi-play
      v-btn.mx-3(icon color="#55a6a1" v-else @click="pauseTimer")
        v-icon mdi-pause
      v-btn.mx-3(icon color="#fe4370" v-if="current.length > 0" @click="finishTimer(true)")
        v-icon mdi-skip-next
  v-col(cols="12")
    h1 {{ currentText }}
</template>

<script setup>
import { computed, ref } from 'vue'
import { storeToRefs } from 'pinia'
import { useListStore } from '@/stores/list'
import { useSettingsStore } from '@/stores/settings'

const list = useListStore()
const { countdown, start, finish } = list

const settings = useSettingsStore()
const { current, items, timeleft } = storeToRefs(list)
const { selectedAlarmFile } = storeToRefs(settings)

const currentText = computed(() => {
  return current.value.length > 0 ? current.value : items.value.length > 0 ? '點擊開始' : '沒有事項'
})
const timeText = computed(() => {
  const m = Math.floor(timeleft.value / 60).toString().padStart(2, '0')
  const s = (timeleft.value % 60).toString().padStart(2, '0')
  return m + ':' + s
})

let timer
// 0 = 停止
// 1 = 倒數中
// 2 = 暫停
const status = ref(0)

const pauseTimer = () => {
  status.value = 2
  clearInterval(timer)
}

const finishTimer = (skip) => {
  clearInterval(timer)
  status.value = 0
  finish()
  if (!skip) {
    const audio = new Audio()
    audio.src = selectedAlarmFile.value
    audio.play()
  }
  if (items.value.length > 0) {
    startTimer()
  }
}

const startTimer = () => {
  if (status.value === 0 && items.value.length > 0) {
    start()
  }
  if (current.value.length > 0) {
    status.value = 1
    timer = setInterval(() => {
      countdown()
      if (timeleft.value === 0) {
        finishTimer(false)
      }
    }, 1000)
  }
}

</script>
