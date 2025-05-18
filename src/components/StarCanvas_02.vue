<template>
  <div ref="container" style="width:100%; height:50vh; touch-action: none;">
    <div style="text-align:center; margin-bottom:10px;">
      <label>観測時刻: {{ hour }} 時</label><br />
      <input type="range" min="0" max="23" v-model="hour" /><br />
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
const showGrid = ref(false)
const stars = ref([])

// 回転（赤経）のオフセット（ラジアン）
// hourとは独立して操作可能にする
const rotationOffsetRad = ref(0)

// ズーム倍率（1.0が標準）
const zoomScale = ref(1.0)
const minZoom = 0.5
const maxZoom = 40

// マウス・タッチ操作状態管理
let isDragging = false
let lastX = 0

// 星の色計算（bv_colorに基づく）
function getStarColor(bv) {
  const b = parseFloat(bv)
  if (isNaN(b)) return 'white'
  if (b < -0.33) return '#9bb0ff' // 青白（O型）
  if (b < 0.00) return '#aabfff' // 青（B型）
  if (b < 0.30) return '#cad7ff' // 青白（A型）
  if (b < 0.58) return '#f8f7ff' // 白（F型）
  if (b < 0.81) return '#fff4e8' // 黄白（G型）
  if (b < 1.40) return '#ffd2a1' // 橙（K型）
  return '#ffcc6f'               // 赤（M型）
}

// 赤経赤緯を極座標に変換
function raDecToPolarXY(ra, dec, width, height, rotationRad, zoom) {
  const centerX = width / 2
  const centerY = height / 2
  const baseRadius = Math.min(width, height) / 2 - 20
  const maxRadius = baseRadius * zoom
  // 赤経は90度ずらして描画（恒星時補正＋操作回転）
  const theta = (((ra - 90) / 360) * 2 * Math.PI) - rotationRad
  const radius = ((90 - dec) / 180) * maxRadius
  const x = centerX + radius * Math.cos(theta)
  const y = centerY + radius * Math.sin(theta)
  return { x, y }
}

// 赤経・赤緯グリッド描画（赤色）
function drawGrid(ctx, width, height, rotationRad, zoom) {
  const centerX = width / 2
  const centerY = height / 2
  const baseRadius = Math.min(width, height) / 2 - 20
  const maxRadius = baseRadius * zoom

  ctx.strokeStyle = 'rgba(255, 0, 0, 0.6)'
  ctx.fillStyle = 'rgba(255, 0, 0, 0.9)'
  ctx.font = '12px sans-serif'
  ctx.textAlign = 'center'
  ctx.textBaseline = 'middle'

  // 赤緯線（-60〜60度、30度間隔）
  for (let dec = -60; dec <= 60; dec += 30) {
    const radius = ((90 - dec) / 180) * maxRadius
    ctx.beginPath()
    ctx.arc(centerX, centerY, radius, 0, Math.PI * 2)
    ctx.stroke()

    // ラベル：左上に少しオフセット
    ctx.fillText(`${dec}°`, centerX - radius / Math.sqrt(2), centerY - radius / Math.sqrt(2))
  }

  // 赤経線（0〜330度、30度間隔）
  for (let ra = 0; ra < 360; ra += 30) {
    const theta = (((ra - 90) / 360) * 2 * Math.PI) - rotationRad
    const x = centerX + maxRadius * Math.cos(theta)
    const y = centerY + maxRadius * Math.sin(theta)

    ctx.beginPath()
    ctx.moveTo(centerX, centerY)
    ctx.lineTo(x, y)
    ctx.stroke()

    // ラベル：線の先端の少し外側
    const labelX = centerX + (maxRadius + 15) * Math.cos(theta)
    const labelY = centerY + (maxRadius + 15) * Math.sin(theta)
    ctx.fillText(`${ra}°`, labelX, labelY)
  }
}

// 星を描画
function drawStars() {
  if (!canvas.value) return
  const ctx = canvas.value.getContext('2d')
  const width = canvas.value.width
  const height = canvas.value.height
  const centerX = width / 2
  const centerY = height / 2
  const baseRadius = Math.min(width, height) / 2 - 20
  const maxRadius = baseRadius * zoomScale.value

  // 現在の時間による回転（hourベース）
  const rotationHourRad = (hour.value * 15 / 360) * 2 * Math.PI
  // 操作による回転オフセットと合成
  const rotationRad = rotationHourRad + rotationOffsetRad.value

  // 背景黒塗りつぶし
  ctx.fillStyle = 'black'
  ctx.fillRect(0, 0, width, height)

  // 外枠と方角（白）
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

  // 赤経・赤緯グリッド
  if (showGrid.value) {
    drawGrid(ctx, width, height, rotationRad, zoomScale.value)
  }

  // 星の描画
  stars.value.forEach(star => {
    const ra = parseFloat(star.ra_deg)
    const dec = parseFloat(star.dec_deg)
    const mag = parseFloat(star.vmag)
    const bv = star.bv_color
    if (isNaN(ra) || isNaN(dec) || isNaN(mag)) return

    const { x, y } = raDecToPolarXY(ra, dec, width, height, rotationRad, zoomScale.value)
    const radiusBase = Math.max(0.5, 4.5 - mag)
    // 明るい星の最大サイズ制限
    const radius = Math.min(radiusBase, 5)

    ctx.beginPath()
    ctx.arc(x, y, radius, 0, Math.PI * 2)
    ctx.fillStyle = getStarColor(bv)
    ctx.fill()
  })
}

// キャンバスのリサイズ対応
function resizeCanvas() {
  if (!canvas.value || !container.value) return
  canvas.value.width = container.value.clientWidth
  canvas.value.height = container.value.clientHeight
  drawStars()
}

// マウスドラッグ開始
function onPointerDown(event) {
  isDragging = true
  lastX = event.clientX ?? event.touches?.[0]?.clientX
  event.preventDefault()
}

// マウスドラッグ終了
function onPointerUp(event) {
  isDragging = false
  event.preventDefault()
}

// マウスドラッグ移動
function onPointerMove(event) {
  if (!isDragging) return
  const currentX = event.clientX ?? event.touches?.[0]?.clientX
  if (currentX === undefined) return
  const dx = currentX - lastX
  lastX = currentX

  // ドラッグのX移動量を回転に変換（感度調整）
  const rotationSpeed = 0.005
  rotationOffsetRad.value += dx * rotationSpeed

  drawStars()
  event.preventDefault()
}

// ホイールズーム
function onWheel(event) {
  event.preventDefault()
  const delta = -event.deltaY * 0.001
  zoomScale.value = Math.min(maxZoom, Math.max(minZoom, zoomScale.value + delta))
  drawStars()
}

// タッチピンチズーム対応用
let lastDistance = null
function onTouchMove(event) {
  if (event.touches.length === 2) {
  event.preventDefault()
const t1 = event.touches[0]
const t2 = event.touches[1]
const dx = t2.clientX - t1.clientX
const dy = t2.clientY - t1.clientY
const distance = Math.sqrt(dx * dx + dy * dy)
  
  if (lastDistance !== null) {
  const zoomChange = (distance - lastDistance) * 0.005
  zoomScale.value = Math.min(maxZoom, Math.max(minZoom, zoomScale.value + zoomChange))
  drawStars()
}
lastDistance = distance
} else if (event.touches.length === 1 && isDragging) {
// 1本指ドラッグで回転操作も
onPointerMove(event)
}
}
   

// タッチ終了で距離リセット
function onTouchEnd(event) {
if (event.touches.length < 2) {
lastDistance = null
}
if (event.touches.length === 0) {
isDragging = false
}
}

watch([hour, showGrid], drawStars)

onMounted(async () => {
// 星データ読み込み
const res = await fetch('/star-data.json')
stars.value = await res.json()

resizeCanvas()
window.addEventListener('resize', resizeCanvas)

// マウス・タッチイベント設定
const c = canvas.value
c.addEventListener('pointerdown', onPointerDown)
c.addEventListener('pointerup', onPointerUp)
c.addEventListener('pointercancel', onPointerUp)
c.addEventListener('pointerleave', onPointerUp)
c.addEventListener('pointermove', onPointerMove)
c.addEventListener('wheel', onWheel, { passive: false })

c.addEventListener('touchmove', onTouchMove, { passive: false })
c.addEventListener('touchend', onTouchEnd)
c.addEventListener('touchcancel', onTouchEnd)
})

onBeforeUnmount(() => {
window.removeEventListener('resize', resizeCanvas)

const c = canvas.value
c.removeEventListener('pointerdown', onPointerDown)
c.removeEventListener('pointerup', onPointerUp)
c.removeEventListener('pointercancel', onPointerUp)
c.removeEventListener('pointerleave', onPointerUp)
c.removeEventListener('pointermove', onPointerMove)
c.removeEventListener('wheel', onWheel)

c.removeEventListener('touchmove', onTouchMove)
c.removeEventListener('touchend', onTouchEnd)
c.removeEventListener('touchcancel', onTouchEnd)
})
</script>

<style scoped> /* 必要に応じて追加 */ </style>


