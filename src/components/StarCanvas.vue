<template>
  <div ref="container" style="width:100%; height:50vh;">
    <div style="text-align:center; margin-bottom:10px;">
      <label>観測時刻: {{ hour }} 時</label><br />
      <input type="range" min="0" max="23" v-model="hour" /><br />
 
      <label>中心赤経: {{ centerRA }}°</label>
      <input type="range" min="0" max="360" step="1" v-model="centerRA" /><br />
 
      <label>中心赤緯: {{ centerDec }}°</label>
      <input type="range" min="-180" max="180" step="1" v-model="centerDec" /><br />
 
      <label>視野角: {{ fieldOfViewDeg }}°</label>
      <input type="range" min="5" max="360" step="5" v-model="fieldOfViewDeg" /><br />
 
      <button @click="showGrid = !showGrid">
        {{ showGrid ? '赤経・赤緯を非表示' : '赤経・赤緯を表示' }}
      </button>
      
    </div>
    <canvas ref="canvas" style="display:block; width:100%; height:100%;"></canvas>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue'

const canvas = ref(null)
const container = ref(null)
const hour = ref(0)
const centerRA = ref(180)
const centerDec = ref(0)
const fieldOfViewDeg = ref(60)
const stars = ref([])
const showGrid = ref(false)

function getStarColor(bv) {
  const b = parseFloat(bv)
  if (isNaN(b)) return '#ffffff'
  if (b < -0.33) return '#9bb0ff'
  if (b < 0.00) return '#aabfff'
  if (b < 0.30) return '#cad7ff'
  if (b < 0.58) return '#f8f7ff'
  if (b < 0.81) return '#fff4e8'
  if (b < 1.40) return '#ffd2a1'
  return '#ffcc6f'
}

function raDecToCanvasXY(ra, dec, width, height, centerRA, centerDec, fieldOfViewDeg) {
  const deg2rad = Math.PI / 180
  const rad2deg = 180 / Math.PI

  const raDiff = (ra - centerRA + 540) % 360 - 180
  const decDiff = dec - centerDec

  const fovRad = fieldOfViewDeg * deg2rad
  const radius = Math.min(width, height) / 2 - 20

  const scale = radius / (fovRad / 2)
  const x = width / 2 + raDiff * deg2rad * scale
  const y = height / 2 - decDiff * deg2rad * scale

  return { x, y }
}





function drawGrid(ctx, width, height) {
  ctx.strokeStyle = 'rgba(0,255,0,0.4)'
  ctx.lineWidth = 0.5
  ctx.font = '12px sans-serif'
  ctx.fillStyle = 'rgba(0,255,0,0.7)'
  ctx.textAlign = 'center'
  ctx.textBaseline = 'top'

  const raStep = 30 // 赤経線（度単位、30度ごと = 2h）
  const decStep = 30 // 赤緯線（度単位）

  // 赤経線（RAを固定、Decを変化）
  for (let ra = 0; ra < 360; ra += raStep) {
    ctx.beginPath()
    let started = false
    let firstXY = null

    for (let dec = -90; dec <= 90; dec += 2) {
      const { x, y } = raDecToCanvasXY(ra, dec, width, height, centerRA.value, centerDec.value, fieldOfViewDeg.value)
      if (!started) {
        ctx.moveTo(x, y)
        started = true
        firstXY = { x, y }
      } else {
        ctx.lineTo(x, y)
      }
    }
    ctx.stroke()

    // ラベル（RA）を一番上の点に表示（RAは時間単位に変換）
    if (firstXY) {
      const raHours = ((ra / 15) + 24) % 24 // 赤経を0〜23hで表示
      ctx.fillText(`${raHours}h`, firstXY.x, 5)
    }
  }

 // 赤緯線（Decを固定、RAを変化）
ctx.textAlign = 'right'
ctx.textBaseline = 'middle'

for (let dec = -60; dec <= 60; dec += decStep) {
  ctx.beginPath()
  let started = false
  let labelXY = null

  for (let ra = 0; ra <= 360; ra += 2) {
    const { x, y } = raDecToCanvasXY(ra, dec, width, height, centerRA.value, centerDec.value, fieldOfViewDeg.value)
    if (!started) {
      ctx.moveTo(x, y)
      started = true
      labelXY = { x, y }
    } else {
      ctx.lineTo(x, y)
    }
  }
  ctx.stroke()

  // ラベル（Dec）を線のすぐ左に表示
  if (labelXY) {
    ctx.font = '12px sans-serif'
    ctx.fillText(`${dec}°`, labelXY.x - 6, labelXY.y)
  }
}

}









function drawStars() {
  const ctx = canvas.value.getContext('2d')
  const width = canvas.value.width
  const height = canvas.value.height

  ctx.fillStyle = 'black'
  ctx.fillRect(0, 0, width, height)

  const rotationDeg = hour.value * 15
  const rotationRad = (rotationDeg / 360) * 2 * Math.PI

  // グリッドの描画（表示ONの場合のみ）
  if (showGrid.value) {
    drawGrid(ctx, width, height)
  }

  // 視野円の描画
  ctx.beginPath()
  ctx.strokeStyle = 'white'
  const radius = Math.min(width, height) / 2 - 20
  ctx.arc(width / 2, height / 2, radius, 0, Math.PI * 2)
  ctx.stroke()

  // 星の描画…
  stars.value.forEach(star => {
    const ra = parseFloat(star.ra_deg)
    const dec = parseFloat(star.dec_deg)
    const mag = parseFloat(star.vmag)
    const bv = parseFloat(star.bv_color)
    if (isNaN(ra) || isNaN(dec) || isNaN(mag)) return

    const raDiff = ((ra - centerRA.value + 540) % 360) - 180
    const decDiff = dec - centerDec.value
    const distance = Math.sqrt(raDiff * raDiff + decDiff * decDiff)

    const limitDeg = fieldOfViewDeg.value / 2
    if (distance > limitDeg) return

    const { x, y } = raDecToCanvasXY(
      ra, dec,
      width, height,
      centerRA.value, centerDec.value,
      fieldOfViewDeg.value
    )

    const baseSize = 1.5
    const scaleFactor = 0.3
    const minSize = 0.5
    const maxSize = 4.0
    const radius = Math.max(minSize, Math.min(maxSize, baseSize - scaleFactor * mag))
    const color = getStarColor(bv)
    const alpha = Math.min(1.0, Math.max(0.3, 1.5 - mag / 2))

    ctx.beginPath()
    ctx.arc(x, y, radius, 0, Math.PI * 2)
    ctx.fillStyle = hexToRgba(color, alpha)
    ctx.fill()
  })
}








function hexToRgba(hex, alpha = 1) {
  const r = parseInt(hex.slice(1, 3), 16)
  const g = parseInt(hex.slice(3, 5), 16)
  const b = parseInt(hex.slice(5, 7), 16)
  return `rgba(${r}, ${g}, ${b}, ${alpha})`
}

function resizeCanvas() {
  if (!canvas.value || !container.value) return
  canvas.value.width = container.value.clientWidth
  canvas.value.height = container.value.clientHeight
  drawStars()
}

onMounted(async () => {
  try {
    const response = await fetch('/star-data.json')
    const data = await response.json()
    stars.value = data.filter(star => parseFloat(star.vmag) <= 6.0)
    resizeCanvas()
    window.addEventListener('resize', resizeCanvas)
  } catch (e) {
    console.error('星データの読み込みに失敗しました', e)
  }
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', resizeCanvas)
})

watch([hour, showGrid, centerRA, centerDec, fieldOfViewDeg], () => {
  drawStars()
})


</script>
