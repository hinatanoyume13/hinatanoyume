<script setup>

import Observation01 from './components/none.vue'
import StarMap01 from './components/StarCanvas_vol4_04_UI調整.vue'
import Status01 from './components/none.vue'




import './style_add.css'
</script>

<template>

  <div class="app-container">
    <!-- 観測画面  -->
    <StarMap01 />
  </div>
  <br />
  <br />
  
    <div class="app-container">
    <!-- 観測画面  -->
    <Observation01 />
  </div>
  <br />
  <br />

</template>

<style scoped>
/* App.vue 側では特別なレイアウトは不要 */
</style>
