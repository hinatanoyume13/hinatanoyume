<template>
 <div class="display-screen01">
    <label>星図 cos補正あり</label>
    
    
   
  <!-- レイアウトテスト-->
  <div class="topUI-out-container">
  
   <div class="topUI-container01" >
      <div class="topUI-panel01" >
      <button type="button">等級</button>
      </div>
    </div>

   <div class="topUI-container02" >
      <div class="topUI-panel02" >
      <button type="button">導入</button>
      </div>
    </div>
   </div>
  <!-- レイアウトテスト ここまで-->





  <div class="star-out-container">


    <!-- キャンバス -->
    <div class="star-container01" ref="container">
      <canvas class="star-canvas" ref="canvas" ></canvas>
    </div>
    
    
    <!-- UI -->
    <div class="star-container02" >
      <div class="ui-panel" >
      
       <div class="input-row">
        <label>赤経(RA):</label>
       <input type="time"  step="1" v-model="raTime" @input="updateRaDegFromTime"/>
       </div>

       
		<div class="input-row">
        <label>赤経(度)</label>
        <input type="number" min="0" max="360" step="1" v-model="centerRA" />
       </div>       
       
        
       <div class="input-row">
        <label>赤緯(Dec):</label>
        <input type="number" min="-90" max="90" step="1" v-model="centerDec" />
       </div>


       <div class="input-row">
        <label>方位(Az):</label>
        <input type="number" min="-180" max="180" step="1" v-model="test01" />
       </div>

 
       <div class="input-row">
        <label>高度(Alt):</label>
        <input type="number" min="-180" max="180" step="1" v-model="test02" />
       </div>

 
 
<!-- 
 		<div class="input-row">
        <label>観測時刻:</label>
        <input type="number" min="0" max="23" v-model="hour" /><br />
 		</div>
-->
 
 		<div class="input-row">
        <label>視野角: </label>
        <input type="number" min="5" max="720" step="5" v-model="fieldOfViewDeg" />
		</div>

        <button @click="showGrid = !showGrid">
          {{ showGrid ? '赤経・赤緯を非表示' : '赤経・赤緯を表示' }}
        </button>
        
      </div>
    </div>
    
  </div>
  
  
  <!-- レイアウトテスト -->
  <div class="underUI-out-container">
  
   <div class="underUI-container01" >
      <div class="underUI-panel01" >
      <button type="button">スコープ切替</button>
      <button type="button">星図切替</button>
      </div>
    </div>

   <div class="underUI-container02" >
      <div class="underUI-panel02" >
      
      <button type="button">太陽系</button>
      <button type="button">恒星</button>
      <button type="button">非恒星状天体</button>
      </div>
    </div>
   </div>
  <!-- レイアウトテスト ここまで-->
  
  
  
 </div>
</template>



<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue'

const aspectRatio = 4 / 3  // ← 比率を固定

const canvas = ref(null)
const container = ref(null)

const hour = ref(0)

// 初期値（12h = 180°）
//const raHour = ref(12) 

// === 赤経（RA）関連 ===
const raTime = ref("12:00:00") // 初期表示
const centerRA = ref(180)       // 内部的には度で管理（12h → 180°）
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


// 星の「赤経・赤緯（RA・Dec）」を、画面上（キャンバス）のX・Y座標に変換する処理
function raDecToCanvasXY(ra, dec, width, height, centerRA, centerDec, fieldOfViewDeg) {

  //度（°）とラジアン（rad）を相互変換
  const deg2rad = Math.PI / 180
  const rad2deg = 180 / Math.PI


  //表示の中心 (centerRA, centerDec)
  //その中心からのズレ量（差）を計算   -180〜+180° の範囲に補正
  
  
  //RA方向を左右反転させる（夜空の見え方に合わせる）
  const raDiff = (centerRA - ra + 540) % 360 - 180
  //const raDiff = (ra - centerRA + 540) % 360 - 180

  const decDiff = dec - centerDec
  


  //表示スケール（視野角）を決定
  const fovRad = fieldOfViewDeg * deg2rad
  
  
  // ★ ここを修正：横方向・縦方向でそれぞれスケールを決める
  const scaleX = width / (fovRad)
  const scaleY = height / (fovRad)

  // 赤緯による横方向のcos補正
  const cosDec = Math.cos(dec * deg2rad)

  // ★ スケールを独立して扱うことで、長方形いっぱいに表示される
  const x = width / 2 + raDiff * deg2rad * scaleX * cosDec
  const y = height / 2 - decDiff * deg2rad * scaleY
  
  
  
  
  //const radius = Math.min(width, height) / 2 - 20
  //const scale = radius / (fovRad / 2)

  //実際の座標計算(補正なし旧ver)
  //const x = width / 2 + raDiff * deg2rad * scale

  // ★ 修正ポイント：RA方向の歪みを補正（緯度のcosでスケーリング）
  //  const cosDec = Math.cos(centerDec * deg2rad)
  //  const x = width / 2 + raDiff * deg2rad * scale * cosDec
    
  //  const cosDec = Math.cos(dec * deg2rad)
  //  const x = width / 2 + raDiff * deg2rad * scale * cosDec

  //const y = height / 2 - decDiff * deg2rad * scale

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

  // 視野円の描画→不要
  //ctx.beginPath()
  //ctx.strokeStyle = 'white'
  //const radius = Math.min(width, height) / 2 - 20
  //ctx.arc(width / 2, height / 2, radius, 0, Math.PI * 2)
  //ctx.stroke()

  // 星の描画…
  stars.value.forEach(star => {
    const ra = parseFloat(star.ra_deg)
    const dec = parseFloat(star.dec_deg)
    const mag = parseFloat(star.vmag)
    const bv = parseFloat(star.bv_color)
    if (isNaN(ra) || isNaN(dec) || isNaN(mag)) return

    const raDiff = ((ra - centerRA.value + 540) % 360) - 180
    const decDiff = dec - centerDec.value


	//「円の視野の外にある星は描かない」
    //const distance = Math.sqrt(raDiff * raDiff + decDiff * decDiff)
    //const limitDeg = fieldOfViewDeg.value / 2
    //if (distance > limitDeg) return

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

  const parentWidth = container.value.clientWidth
  const newWidth = parentWidth
  const newHeight = parentWidth / aspectRatio  // ← 比率を維持

  canvas.value.width = newWidth
  canvas.value.height = newHeight

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




// 時刻 → 度 に変換
function updateRaDegFromTime() {
  const [h, m, s] = raTime.value.split(":").map(Number)
  const hour = h + m / 60 + s / 3600
  centerRA.value = hour * 15 // 1時間＝15°
}
// 度 → 時刻 に変換（双方向反映）
watch(centerRA, (deg) => {
  const hour = deg / 15
  const h = Math.floor(hour)
  const m = Math.floor((hour - h) * 60)
  const s = Math.floor(((hour - h) * 60 - m) * 60)
  const pad = (n) => n.toString().padStart(2, "0")
  raTime.value = `${pad(h)}:${pad(m)}:${pad(s)}`
})


watch([raTime, hour, showGrid, centerRA, centerDec, fieldOfViewDeg], () => {
  drawStars()
})




</script>
