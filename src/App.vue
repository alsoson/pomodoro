<template lang="pug">
v-app
  v-app-bar(color="#d11f47")
    v-app-bar-title.d-flex.justify-content-center.align-center.align-self-center
      //-v-img(src='https://cdn-icons-png.flaticon.com/512/7329/7329721.png')
      h3.d-inline-block.ms-3 番茄鐘
    v-spacer
    v-btn(icon to='/')
      v-icon(large) mdi-timer
    v-btn(icon to='/list')
      v-icon mdi-format-list-bulleted
    v-btn(icon to='/settings')
      v-icon mdi-cog
    v-btn(icon v-if="notify" @click="toggleNotify")
      v-icon mdi-bell
    v-btn(icon v-else @click="toggleNotify")
      v-icon mdi-bell-off
  v-main
    v-container
      router-view(v-slot="{ Component }")
        keep-alive(include="HomeView")
          component(:is="Component")

  v-img.tomato(src="https://github.com/alsoson/pomodoro/blob/master/src/assets/images/tomato.png?raw=true")
</template>

<script setup>
import { storeToRefs } from 'pinia'
import { useListStore } from '@/stores/list'
import { onMounted } from "vue"

onMounted(() => {

  gsap.to('.tomato', {
    duration: 10,
    rotate:1070,
    xPercent: 3730,
    yoyo: true,
    loop:true,
    repeat:-1,
    ease:'easeOut',
  });
})
const list = useListStore()
const { notify } = storeToRefs(list)
const { toggleNotify } = list



</script>
