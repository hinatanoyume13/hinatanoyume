<template>

 <!-- width:100%; height:50vh; これでindex.htmlとあわせて丁度、縦横のレスポンシブルになる -->
  <div ref="container" style="width:100%; height:50vh;">
    <div style="text-align:center; margin-bottom:10px;">
      <label>観測時刻: {{ hour }} 時</label><br />
      <input type="range" min="0" max="23" v-model="hour" />
    </div>
    <canvas ref="canvas" style="display:block; width:100%; height:100%;"></canvas>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue'

const canvas = ref(null)
const container = ref(null)
const hour = ref(0)

const stars = [
  { ra: 0, dec: 0 },
  { ra: 30, dec: 10 },
  { ra: 60, dec: -20 },
  { ra: 120, dec: 45 },
  { ra: 200, dec: -45 },
  { ra: 330, dec: 0 }
]

function drawStars() {
  const ctx = canvas.value.getContext('2d')
  const width = canvas.value.width
  const height = canvas.value.height
  const centerX = width / 2
  const centerY = height / 2
  const maxRadius = Math.min(width, height) / 2 - 20

  const rotationDeg = hour.value * 15
  const rotationRad = (rotationDeg / 360) * 2 * Math.PI

  // 背景
  ctx.fillStyle = 'black'
  ctx.fillRect(0, 0, width, height)

  // 外枠と方角
  ctx.strokeStyle = 'white'
  ctx.beginPath()
  ctx.arc(centerX, centerY, maxRadius, 0, Math.PI * 2)
  ctx.stroke()

  ctx.fillStyle = 'white'
  ctx.font = '16px sans-serif'
  ctx.textAlign = 'center'
  ctx.textBaseline = 'middle'
  ctx.fillText('N', centerX, centerY - maxRadius - 10)
  ctx.fillText('S', centerX, centerY + maxRadius + 10)
  ctx.fillText('E', centerX + maxRadius + 15, centerY)
  ctx.fillText('W', centerX - maxRadius - 15, centerY)

  // 星の描画
  ctx.fillStyle = 'white'
  stars.forEach(star => {
    const { x, y } = raDecToPolarXY(star.ra, star.dec, width, height, rotationRad)
    ctx.beginPath()
    ctx.arc(x, y, 3, 0, Math.PI * 2)
    ctx.fill()
  })
}

function raDecToPolarXY(ra, dec, width, height, rotationRad) {
  const centerX = width / 2
  const centerY = height / 2
  const maxRadius = Math.min(width, height) / 2 - 20
  const theta = (((ra - 90) / 360) * 2 * Math.PI) - rotationRad
  const radius = ((90 - dec) / 180) * maxRadius
  const x = centerX + radius * Math.cos(theta)
  const y = centerY + radius * Math.sin(theta)
  return { x, y }
}

// キャンバスサイズ調整
function resizeCanvas() {
  if (!canvas.value || !container.value) return
  canvas.value.width = container.value.clientWidth
  canvas.value.height = container.value.clientHeight
  drawStars()
}

// 初期化
onMounted(() => {
  resizeCanvas()
  window.addEventListener('resize', resizeCanvas)
})

// クリーンアップ
onBeforeUnmount(() => {
  window.removeEventListener('resize', resizeCanvas)
})

// 時刻変更時の再描画
watch(hour, () => {
  drawStars()
})
</script>
