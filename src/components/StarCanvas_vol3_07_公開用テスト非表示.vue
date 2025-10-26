<template>



<!-- 観測画面レイアウト-->

 <div class="display-screen01">
    <label>スライド</label>
   
  <!-- 上段レイアウトテスト-->
  <!-- 上段レイアウトテスト ここまで-->



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
 		 <input type="text" v-model="raTime" placeholder="12:00:00" />
		</div>

		<div class="input-row">
 		 <label>赤緯(Dec):</label>
 		 <input type="text" v-model="decDms" placeholder="+12:30:00" />
		</div>
		

		
	



		<div class="input-row">
		  <label>方位(Az):</label>
		<input type="text" v-model="test01" placeholder="00:00:00" readonly />
		</div>

		<div class="input-row">
  		<label>高度(Alt):</label>
  		<input type="text" v-model="test02" placeholder="+00:00:00" readonly />
		</div>

 
 
<!-- 
 		<div class="input-row">
        <label>観測時刻:</label>
        <input type="number" min="0" max="23" v-model="hour" /><br />
 		</div>
-->

        
      </div>
    </div>
    
  </div>
  
  
  <!-- レイアウトテスト -->
  <div class="underUI-out-container">
    
   <div class="underUI-container01" >
      
      <div class="underUI-panel01" >
      <button type="button">スコープ切替</button>
      <button type="button">スナップ</button>
      </div>
      
      
    </div>

   <div class="underUI-container02" >
      <div class="underUI-panel01" >
      
      <button type="button">感度[0]</button>
      <button type="button">露出時間[0]</button>

      </div>
    </div>
        
    
   </div>
      		
   
  
  
  
 </div>

<!-- 観測画面　ここまで-->



<!-- 星図レイアウト　　　　　　　-->

 <div class="display-screen01">
    <label>星図 (vol.3.07)</label>
   






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
 		 <input type="text" v-model="raTime" placeholder="12:00:00" />
		</div>

		<div class="input-row">
 		 <label>赤緯(Dec):</label>
 		 <input type="text" v-model="decDms" placeholder="+12:30:00" />
		</div>
		

		<button type="button" @click="applyRaDec">赤経赤緯を反映</button><br />
	



		<div class="input-row">
		  <label>方位(Az):</label>
		<input type="text" v-model="test01" placeholder="00:00:00" readonly />
		</div>

		<div class="input-row">
  		<label>高度(Alt):</label>
  		<input type="text" v-model="test02" placeholder="+00:00:00" readonly />
		</div>

 
 
<!-- 
 		<div class="input-row">
        <label>観測時刻:</label>
        <input type="number" min="0" max="23" v-model="hour" /><br />
 		</div>
-->

        
      </div>
    </div>
    
  </div>
  
  
    <!-- 中段レイアウト-->
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
  <!-- 中段レイアウト ここまで-->
  
  
  
  
  
  
  
  
  <!-- 下段レイアウト -->
  <div class="underUI-out-container">
    
   <div class="underUI-container01" >
   
   
     <div class="underUI-panel01" >
            
      <!-- ▼スコープ切替ボタン -->
      <button @click="toggleScopeMenu">スコープ切替</button>
      
        <!-- スコープメニュー -->
  		<div v-if="showScopeMenu" class="dropdown-menu">
    		<ul>
      		 <li @click="selectScope(1)">サブスコープ（視野角1°）</li>
      		 <li @click="selectScope(30)">広角スコープ（視野角30°）</li>
    		</ul>
  		</div>
	      
      <button type="button">星図切替</button>
      
     </div>
      
      
      
      
    </div>

   <div class="underUI-container02" >
      <div class="underUI-panel02" >
      
      
      <input type="text" v-model="starname" placeholder="天体の名前" readonly />
      
      <button type="button">太陽系</button>
      
      <button @click="toggleStarMenu">恒星</button>
      
      <!-- 恒星メニュー -->
		<div v-if="showStarMenu" class="dropdown-menu">
		  <ul>
		    <li v-for="fixed in fixedStars" :key="fixed.name" @click="selectFixedStar(fixed)">
              {{ fixed.name }}
            </li>
		  </ul>
		</div>
      
      
      
      <button @click="toggleMenu">非恒星状天体</button>
		<!-- メニュー（表示／非表示切り替え）-->
      <div v-if="showMenu" class="dropdown-menu">
        <ul>
          <li v-for="obj in nonStellarObjects" :key="obj.name" @click="selectObject(obj)">
            {{ obj.name }}
          </li>
        </ul>
      </div>
      
      
      
      
      

      </div>
    </div>
    
    
    
    
    
    
   </div>
  
  <!-- 
   
   		<label>テスト用　項目</label>
   
   		<div class="input-row">
        <label>赤経(度)</label>
        <input type="number" min="0" max="360" :step="stepValue" v-model="centerRA" />
        </div> 
       
        <div class="input-row">
        <label>赤緯(Dec):</label>
        <input type="number" min="-90" max="90" :step="stepValue" v-model="centerDec" />
        </div>
          
    
 		<div class="input-row">
        <label>視野角: </label>
        <input type="number" min="0" max="720" :step="stepValue" v-model="fieldOfViewDeg" />
		</div>


		<div class="input-row">
		<label>値のステップ: </label>
		<input type="number" min="0" max="10" step="0.01" v-model.number="stepValue" />
		</div>
   
   		<div class="input-row">
        <button @click="showGrid = !showGrid">
          {{ showGrid ? '赤経・赤緯を非表示' : '赤経・赤緯を表示' }}
        </button>
        </div>
   
   <div class="time-display">
    <p>現在時刻（日本時間）: {{ currentTime }}</p>
    <p>現在地: {{ currentLocation.name }}</p>
    <p>緯度: {{ currentLocation.lat }}°, 経度: {{ currentLocation.lon }}°</p>
  </div>
      
      -->
      
  <!-- 下段レイアウト ここまで-->
   
 </div>
<!-- 星図レイアウト　ここまで--> 
 
 
 
 
 
 
<!-- ステータス用レイアウト--> 
 
  <div class="status-screen01">
    <label>ステータス</label>
   
		<!-- 上段レイアウトテスト-->
		<!-- 上段レイアウトテスト ここまで-->

  
  
  <!-- レイアウトテスト -->
  <div class="statusUI-out-container">
    
   <div class="statusUI-container01" >
      
      <div class="statusUI-panel01" >
      <button type="button">ログアウト</button>
      <label>残り時間(秒):</label>
      <input type="text" v-model="logcount" placeholder="000" readonly />
      </div>
      
      
    </div>

   <div class="statusUI-container02" >
   
      <div class="statusUI-panel02" >
 
 	  <textarea v-model="statuslog" placeholder="1>送信成功" readonly></textarea> 
 	  
 	  <!--
      <input type="text" v-model="statuslog" placeholder="1>送信成功" readonly />
		-->
      </div>
    </div>
        
    
   </div>
      		
   
  
  
  
 </div>

<!-- ステータス用レイアウト　ここまで--> 

 
 
 
</template>



<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue'




// ===== 恒星メニュー関連 =====
const showStarMenu = ref(false) // 恒星メニュー表示フラグ


const fixedStars = ref([
  { name: 'シリウス（おおいぬ座α）', ra: '06:45:09', dec: '-16:42:58' },
  { name: 'ベテルギウス（オリオン座α）', ra: '05:55:10', dec: '+07:24:25' },
  { name: 'プロキオン（こいぬ座α）', ra: '07:39:18', dec: '+05:13:30' },
  { name: 'アルタイル（わし座α）', ra: '19:50:47', dec: '+08:52:06' },
  { name: 'ベガ（こと座α）', ra: '18:36:56', dec: '+38:47:01' },
  { name: 'デネブ（はくちょう座α）', ra: '20:41:25', dec: '+45:16:49' },
  { name: 'スピカ（おとめ座α）', ra: '13:25:11', dec: '-11:09:41' },
  { name: 'レグルス（しし座α）', ra: '10:08:22', dec: '+11:58:02' },
  { name: 'アルデバラン（おうし座α）', ra: '04:35:55', dec: '+16:30:33' },
  { name: 'アンタレス（さそり座α）', ra: '16:29:24', dec: '-26:25:55' }
])

function toggleStarMenu() {
  showStarMenu.value = !showStarMenu.value
}


function selectFixedStar(fixed) {
  raTime.value = fixed.ra
  decDms.value = fixed.dec
  starname.value = fixed.name
  showStarMenu.value = false
}






const starname = ref('')

// メニュー表示フラグ
const showMenu = ref(false)

// 非恒星状天体リスト（例）
const nonStellarObjects = ref([

  { name: 'M31（アンドロメダ銀河）', ra: '00:42:44', dec: '+41:16:09' },
  { name: 'M33（三角座銀河）', ra: '01:33:50', dec: '+30:39:37' },
  { name: 'M1（かに星雲）', ra: '05:34:32', dec: '+22:00:52' },
  { name: 'M42（オリオン大星雲）', ra: '05:35:17', dec: '-05:23:28' },
  { name: 'M45（プレアデス星団）', ra: '03:47:00', dec: '+24:07:00' },
  { name: 'M44（プレセペ星団）', ra: '08:40:24', dec: '+19:59:00' },
  { name: 'M13（ヘルクレス星団）', ra: '16:41:41', dec: '+36:27:37' },
  { name: 'M57（こと座リング星雲）', ra: '18:53:35', dec: '+33:01:45' },
  { name: 'M8（干潟星雲）', ra: '18:03:37', dec: '-24:23:12' },
  { name: 'M20（三裂星雲）', ra: '18:02:42', dec: '-23:01:00' },
  { name: 'M16（わし星雲）', ra: '18:18:48', dec: '-13:48:24' },
  { name: 'M17（オメガ星雲）', ra: '18:20:26', dec: '-16:10:36' },
  { name: 'M27（あれい状星雲）', ra: '19:59:36', dec: '+22:43:16' },
  { name: 'M104（ソンブレロ銀河）', ra: '12:39:59', dec: '-11:37:23' },
  { name: 'M51（子持ち銀河）', ra: '13:29:52', dec: '+47:11:43' },
  { name: 'M101（回転花火銀河）', ra: '14:03:13', dec: '+54:20:57' },
  { name: 'M64（黒眼銀河）', ra: '12:56:43', dec: '+21:41:00' },
  { name: 'NGC 253（ちょうこくしつ座銀河）', ra: '00:47:33', dec: '-25:17:18' },
  { name: 'NGC 5128（ケンタウルスA銀河）', ra: '13:25:28', dec: '-43:01:09' },
  { name: 'NGC 5139（オメガ星団）', ra: '13:26:47', dec: '-47:28:46' },
  { name: 'NGC 104（47トゥカーナ星団）', ra: '00:24:05', dec: '-72:04:52' },
  { name: 'NGC 7000（北アメリカ星雲）', ra: '20:58:54', dec: '+44:19:00' },
  { name: 'NGC 7293（らせん星雲）', ra: '22:29:38', dec: '-20:50:13' },
  { name: 'NGC 2237（ばら星雲）', ra: '06:31:55', dec: '+04:56:34' },
  { name: 'NGC 3372（カリーナ星雲）', ra: '10:45:08', dec: '-59:52:40' },
  { name: 'IC 2602（南のプレアデス）', ra: '10:42:57', dec: '-64:24:00' },
  { name: 'NGC 3115（紡錘銀河）', ra: '10:05:14', dec: '-07:43:06' },
  { name: 'M104（ソンブレロ銀河）', ra: '12:39:59', dec: '-11:37:23' },
  { name: 'LMC（大マゼラン雲）', ra: '05:23:34', dec: '-69:45:22' },
  { name: 'SMC（小マゼラン雲）', ra: '00:52:38', dec: '-72:48:01' }


])

// メニューの開閉
function toggleMenu() {
  showMenu.value = !showMenu.value
}


// 天体を選択したとき
function selectObject(obj) {
  raTime.value = obj.ra
  decDms.value = obj.dec
  showMenu.value = false  // メニューを閉じる
  starname.value = obj.name  // 天体名の反映
}



const aspectRatio = 4 / 3  // ← 比率を固定

const canvas = ref(null)
const container = ref(null)


const test01 = ref('')  // 方位（Az）
const test02 = ref('')  // 高度（Alt）

const hour = ref(0)

const stars = ref([])

// 初期値（12h = 180°）
//const raHour = ref(12) 

// === 赤経（RA）関連 ===
const raTime = ref("12:00:00") // 初期表示
const centerRA = ref(180)       // 内部的には度で管理（12h → 180°）


// === 赤緯（Dec）関連 ===
const centerDec = ref(0) // 内部的には度
const decDms = ref("+00:00:00") // 表示値（度分秒）


// === 視野 関連 ===
const fieldOfViewDeg = ref(1) // 視野角（初期値）

// メニュー表示状態
const showScopeMenu = ref(false)

// メニュー開閉
const toggleScopeMenu = () => {
  showScopeMenu.value = !showScopeMenu.value
}

// スコープ選択時
const selectScope = (deg) => {
  fieldOfViewDeg.value = deg
  showScopeMenu.value = false
  console.log('視野角を変更しました：', deg)
}




// UI操作関連
const stepValue = ref(1) // ステップ（初期値）
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



  // 星の描画…
  stars.value.forEach(star => {
    const ra = parseFloat(star.ra_deg)
    const dec = parseFloat(star.dec_deg)
    const mag = parseFloat(star.vmag)
    const bv = parseFloat(star.bv_color)
    if (isNaN(ra) || isNaN(dec) || isNaN(mag)) return

    const raDiff = ((ra - centerRA.value + 540) % 360) - 180
    const decDiff = dec - centerDec.value




    const { x, y } = raDecToCanvasXY(
      ra, dec,
      width, height,
      centerRA.value, centerDec.value,
      fieldOfViewDeg.value
    )

	// 星のサイズを決定
    const baseSize = 1.5
    const scaleFactor = 0.3
    const minSize = 0.5
    
    //const maxSize = 4.0
    const maxSize = 10.0  // ← 少し上限を広げる
    const rawScale = 40 / fieldOfViewDeg.value
	const fovScale = Math.max(1, Math.sqrt(rawScale))
	const radius = Math.max(minSize, Math.min(maxSize, (baseSize - scaleFactor * mag) * fovScale))

    
    // ★追加：視野角による拡大補正（視野60°を基準）
	// const fovScale = Math.max(1, 40 / fieldOfViewDeg.value)  
	// 例：視野60°なら倍率1倍、視野6°なら10倍、0.6°なら100倍など
    
    //const radius = Math.max(minSize, Math.min(maxSize, (baseSize - scaleFactor * mag) * fovScale))
    //const radius = Math.max(minSize, Math.min(maxSize, baseSize - scaleFactor * mag))
    
    
    const color = getStarColor(bv)
    const alpha = Math.min(1.0, Math.max(0.3, 1.5 - mag / 2))

    ctx.beginPath()
    ctx.arc(x, y, radius, 0, Math.PI * 2)
    ctx.fillStyle = hexToRgba(color, alpha)
    ctx.fill()
  })
  
  
  //明るさ調整の関数
  applyCanvasBrightnessPreserveColor(ctx, width, height, 20.0)
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



/* ========== RA / Dec の変換関数 ========== */

// HH:MM[:SS] 文字列を度に変換（RA: 0h - 24h -> 0° - 360°）
function hmsToDeg(hmsStr) {
  if (!hmsStr) return null
  // 空白除去
  const s = hmsStr.trim()
  // allow formats like "12:34", "12:34:56", or " 12 : 34 : 56 "
  const parts = s.split(':').map(p => p.trim())
  if (parts.length < 2 || parts.length > 3) return null
  const h = Number(parts[0])
  const m = Number(parts[1])
  const sec = parts.length === 3 ? Number(parts[2]) : 0
  if (![h,m,sec].every(n => Number.isFinite(n))) return null

  // 範囲チェック＆丸め
  let hh = Math.floor(h)
  let mm = Math.floor(m)
  let ss = Math.floor(sec)
  if (hh < 0) hh = 0
  if (mm < 0) mm = 0
  if (ss < 0) ss = 0
  if (mm > 59) mm = 59
  if (ss > 59) ss = 59
  if (hh > 23) hh = hh % 24 // 24h を超える場合は wrap（必要なら clamp に変更）

  const hourVal = hh + mm / 60 + ss / 3600
  return hourVal * 15.0 // 1h = 15°
}

// 度 -> HH:MM:SS 形式（常に 2桁で整形）
function degToHms(deg) {
  const normalized = ((deg % 360) + 360) % 360
  const hourVal = normalized / 15.0
  const h = Math.floor(hourVal)
  const m = Math.floor((hourVal - h) * 60)
  const s = Math.floor(((hourVal - h) * 60 - m) * 60)
  const pad = n => String(n).padStart(2, '0')
  return `${pad(h)}:${pad(m)}:${pad(s)}`
}

/* ====== RA: テキスト入力から内部（度）へ変換 ====== */
function updateRaDegFromTime() {
  // 入力文字列をパースして度にする（不正なら無視）
  const deg = hmsToDeg(raTime.value)
  if (deg === null || Number.isNaN(deg)) {
    // 不正入力：無視する（またはユーザーに警告する実装に変えても良い）
    return
  }
  centerRA.value = deg
}

/* 双方向：中心の degree が変わったら表示文字列を整形 */
watch(centerRA, (deg) => {
  raTime.value = degToHms(deg)
})



//
//

// === 赤緯（Dec）関連 ===
/* DMS文字列 → 度 に変換 */
function dmsToDeg(dmsStr) {
  if (!dmsStr) return 0
  const s = dmsStr.trim()
  const sign = s.startsWith('-') ? -1 : 1
  const cleaned = s.replace('+', '').replace('-', '')
  const parts = cleaned.split(':').map(p => p.trim())
  if (parts.length < 2) return 0
  const d = Number(parts[0]) || 0
  const m = Number(parts[1]) || 0
  const sec = parts.length >= 3 ? Number(parts[2]) || 0 : 0
  return sign * (Math.abs(d) + m / 60 + sec / 3600)
}

/* 度 → DMS文字列 に変換 */
function degToDms(deg) {
  const sign = deg >= 0 ? '+' : '-'
  const absDeg = Math.abs(deg)
  const d = Math.floor(absDeg)
  const m = Math.floor((absDeg - d) * 60)
  const s = Math.floor(((absDeg - d) * 60 - m) * 60)
  const pad = (n) => n.toString().padStart(2, "0")
  return `${sign}${pad(d)}:${pad(m)}:${pad(s)}`
}


/* DMS入力時 → 度に更新 */
function updateDecDegFromDms() {
  const deg = dmsToDeg(decDms.value)
  // 範囲チェック（-90〜+90）してから代入
  if (deg > 90) centerDec.value = 90
  else if (deg < -90) centerDec.value = -90
  else centerDec.value = deg
}




// 度が更新されたら DMS表記も更新
watch(centerDec, (deg) => {
  decDms.value = degToDms(deg)
})


// 度 → 時刻 に変換（双方向反映）
watch(centerRA, (deg) => {
  const hour = deg / 15
  const h = Math.floor(hour)
  const m = Math.floor((hour - h) * 60)
  const s = Math.floor(((hour - h) * 60 - m) * 60)
  const pad = (n) => n.toString().padStart(2, "0")
  raTime.value = `${pad(h)}:${pad(m)}:${pad(s)}`
})




// === 赤経・赤緯 共通反映ボタン ===
function applyRaDec() {
  // ▼ 赤経(RA)
  if (raTime.value) {
    const [h, m, s] = raTime.value.split(":").map(Number)
    const hour = h + m / 60 + s / 3600
    centerRA.value = hour * 15 // 1時間＝15°
  }

  // ▼ 赤緯(Dec)
  if (decDms.value) {
    const sign = decDms.value.startsWith('-') ? -1 : 1
    const parts = decDms.value.replace('+', '').replace('-', '').split(':').map(Number)
    const [d, m, s] = parts
    const deg = d + m / 60 + s / 3600
    centerDec.value = sign * deg
  }

  console.log(`RA=${centerRA.value.toFixed(3)}°, Dec=${centerDec.value.toFixed(3)}°`)

  // ▼ 方位・高度の計算
  const date = new Date()
  const result = computeAzAlt(centerRA.value, centerDec.value, currentLocation.value, date)
  test01.value = result.az  // 方位
  test02.value = result.alt // 高度
}






watch([raTime, hour, showGrid, centerRA, centerDec, fieldOfViewDeg], () => {
  drawStars()
})



//明るさだけ調整の関数　drawstar()の最後に追加した機能
//色と明るさの調整　両立
function applyCanvasBrightnessPreserveColor(ctx, width, height, factor = 1.5) {
  const imageData = ctx.getImageData(0, 0, width, height);
  const data = imageData.data;

  for (let i = 0; i < data.length; i += 4) {
    let r = data[i];
    let g = data[i + 1];
    let b = data[i + 2];

    // RGB → HSL
    let max = Math.max(r, g, b);
    let min = Math.min(r, g, b);
    let l = (max + min) / 2;

    // 輝度だけ factor倍
    l *= factor;
    if (l > 255) l = 255;

    // 色比率を保って RGB に戻す
    if (max !== 0) {
      let scale = l / max;
      r = Math.min(255, r * scale);
      g = Math.min(255, g * scale);
      b = Math.min(255, b * scale);
    }

    data[i] = r;
    data[i + 1] = g;
    data[i + 2] = b;
  }

  ctx.putImageData(imageData, 0, 0);
}










// 現在時刻取得（日本時間）
const currentTime = ref('')


// 観測地データ（緯度・経度）
const locations = {
  japan: { name: 'Tokyo, Japan', lat: 35.6895, lon: 139.6917 },
  newyork: { name: 'New York, USA', lat: 40.7128, lon: -74.0060 },
}

// 現在地の選択（初期は日本）
const currentLocation = ref(locations.japan)


// 現在時刻を更新する関数
function updateTime() {
  const now = new Date()
  // 日本時間（UTC+9）に変換
  const japanTime = new Date(now.getTime() + 9 * 60 * 60 * 1000)
  const formatted = japanTime.toISOString().replace('T', ' ').substring(0, 19)
  currentTime.value = formatted
}


// マウント時に現在時刻をセット
onMounted(() => {
  updateTime()
  // 1秒ごとに更新（任意）
  setInterval(updateTime, 1000)
})


// 度→HH:MM:SS 形式
function degToHmsString(deg) {
  const absDeg = Math.abs(deg)
  const d = Math.floor(absDeg)
  const m = Math.floor((absDeg - d) * 60)
  const s = Math.floor(((absDeg - d) * 60 - m) * 60)
  const pad = (n) => String(n).padStart(2, '0')
  const sign = deg >= 0 ? '' : '-'
  return `${sign}${pad(d)}:${pad(m)}:${pad(s)}`
}

// 現在の方位角・高度を計算して HH:MM:SS 形式で返す
function computeAzAlt(raDeg, decDeg, location, date) {
  const JD = date.getTime() / 86400000 + 2440587.5
  const T = (JD - 2451545.0) / 36525

  let GST = 280.46061837 + 360.98564736629 * (JD - 2451545.0) + 0.000387933 * T ** 2 - (T ** 3) / 38710000
  GST = (GST % 360 + 360) % 360

  let LST = GST + location.lon
  LST = (LST % 360 + 360) % 360

  let HA = (LST - raDeg + 360) % 360
  const haRad = HA * Math.PI / 180
  const decRad = decDeg * Math.PI / 180
  const latRad = location.lat * Math.PI / 180

  const sinAlt = Math.sin(decRad) * Math.sin(latRad) + Math.cos(decRad) * Math.cos(latRad) * Math.cos(haRad)
  const altDeg = Math.asin(sinAlt) * 180 / Math.PI

  const cosAz = (Math.sin(decRad) - Math.sin(altDeg * Math.PI/180) * Math.sin(latRad)) /
                (Math.cos(altDeg * Math.PI/180) * Math.cos(latRad))
  let azDeg = Math.acos(cosAz) * 180 / Math.PI
  if (Math.sin(haRad) > 0) azDeg = 360 - azDeg

  return {
    az: degToHmsString(azDeg),                 // 方位はそのまま
    alt: (altDeg >= 0 ? '+' : '-') + degToHmsString(Math.abs(altDeg))  // 高度に符号を付与
  }
}


</script>
