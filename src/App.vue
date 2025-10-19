<script setup>

import StarCanvas01 from './components/StarCanvas_vol2_00初期案_RA反転.vue'

import StarCanvas02 from './components/StarCanvas_vol2_03視野長方形_RA反転.vue'

import StarCanvas03 from './components/StarCanvas_vol2_10方位高度試作.vue'





import './style_add.css'
</script>

<template>


  <div class="app-container">
    <!-- 画面  -->
    
    <StarCanvas01 />
    
  </div>
  <br />
  <br />


  <div class="app-container">
    <!-- 画面  -->
    
    <StarCanvas02 />
    
  </div>
  <br />
  <br />

  <div class="app-container">
    <!-- 画面  -->
    
    <StarCanvas03 />
    
  </div>
  <br />
  <br />


  
  
  

</template>

<style scoped>
/* App.vue 側では特別なレイアウトは不要 */
</style>
