<template>

<!-- プルダウンメニュー　キャンセルクリック検知用 -->
<div
  v-if="showSolarSystemMenu || showStarMenu || showMenu || showGainMenu || showExposureMenu || showScopeMenu || showScopeMenu02 || showMagnitudeMenu"
  class="click-catcher"
  
  @click="closeAllMenus" ></div>



<!-- 観測　レイアウト-->

 <div class="display-screen01">
    <label>観測 slide (vol.5.01)</label>
   
  <!-- 上段レイアウトテスト-->
  <!-- 上段レイアウトテスト ここまで-->



  <div class="star-out-container">


    <!--観測 キャンバス -->
    
    <div class="star-container01" ref="pictureContainer">
      <canvas class="star-canvas" ref="pictureCanvas"></canvas>
    </div>
        
    
    <!-- UI -->
    <div class="star-container02" >
      <div class="ui-panel" >
      
		<div class="input-row">
 		 <label>赤経(RA):</label>
 		 <input type="text" v-model="raTime_Slide" placeholder="00:00:00" readonly/>
		</div>

		<div class="input-row">
 		 <label>赤緯(Dec):</label>
 		 <input type="text" v-model="decDms_Slide" placeholder="+00:00:00" readonly />
		</div>
		

		
	



		<div class="input-row">
		  <label>方位(Az):</label>
		<input type="text" v-model="canvasAz_Slide" placeholder="00:00:00" readonly />
		</div>

		<div class="input-row">
  		<label>高度(Alt):</label>
  		<input type="text" v-model="canvasAlt_Slide" placeholder="+00:00:00" readonly />
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
  
  
  <!--観測 レイアウトテスト -->
  <div class="underUI-out-container">
    
   <div class="underUI-container01" >
      
      <div class="underUI-panel01" >
      
      
      

      <button @click="toggleScopeMenu02" @click.stop >スコープ({{ currentScope02 }}°)</button>
      
        <div v-if="showScopeMenu02" class="dropdown-menu">
    		<ul>
      		 <li @click="selectScope02(0.05)">メインスコープ（視野角0.05°）</li>
      		 <li @click="selectScope02(1)">サブスコープ（視野角1°）</li>
      		 <li @click="selectScope02(30)">広角スコープ（視野角30°）</li>
    		</ul>
  		</div>
      
      
      
      
      
      
      <button @click="saveAsJpg">スナップ</button>
      </div>
      
      
    </div>

   <div class="underUI-container02" >
      <div class="underUI-panel01" >
      
      
      <button @click="toggleGainMenu" @click.stop >感度[{{ lookButtonGain }}]</button>
      
        <div v-if="showGainMenu" class="dropdown-menu">
    		<ul>
      		 <li @click="selectGainMenu(0)">0</li>
      		 <li @click="selectGainMenu(1)">1</li>
      		 <li @click="selectGainMenu(2)">2</li>
      		 <li @click="selectGainMenu(3)">3</li>
      		 <li @click="selectGainMenu(4)">4</li>
      		 <li @click="selectGainMenu(5)">5</li>
      		 <li @click="selectGainMenu(6)">6</li>
      		 <li @click="selectGainMenu(7)">7</li>
      		 <li @click="selectGainMenu(8)">8</li>
      		 <li @click="selectGainMenu(9)">9</li>
      		 <li @click="selectGainMenu(10)">10</li>
      		 
    		</ul>
  		</div>
      
      
      <button @click="toggleExposureMenu" @click.stop >露出時間[{{ lookButtonExposure }}]</button>
      
        <div v-if="showExposureMenu" class="dropdown-menu">
    		<ul>
      		 <li @click="selectExposureMenu(0)">0</li>
      		 <li @click="selectExposureMenu(1)">1</li>
      		 <li @click="selectExposureMenu(2)">2</li>
      		 <li @click="selectExposureMenu(3)">3</li>
      		 <li @click="selectExposureMenu(4)">4</li>
      		 <li @click="selectExposureMenu(5)">5</li>
      		 <li @click="selectExposureMenu(6)">6</li>
      		 <li @click="selectExposureMenu(7)">7</li>
      		 <li @click="selectExposureMenu(8)">8</li>
      		 <li @click="selectExposureMenu(9)">9</li>

      		 
    		</ul>
  		</div>
      
  

      </div>
    </div>
        
    
   </div>
      		
   
  
  
  
 </div>

<!-- 観測画面　ここまで-->



<!-- デバッグ用レイアウト--> 

 <div v-if="showTestUI">
 <div class="status-screen01">
 <div class="statusUI-out-container">
 <div class="statusUI-container01" >
 <div>
   
   		<label>テスト用</label>
   
		<div class="input-row">
		  <label>赤経(度)</label>
		  <input type="number" min="0" max="360" :step="stepValue" v-model="centerRA" />
		  <button @click.stop="incRA">↑</button>
		  <button @click.stop="decRA">↓</button>
		</div>


		<div class="input-row">
		  <label>赤緯(Dec)</label>
		  <input type="number" min="-90" max="90" :step="stepValue" v-model="centerDec" />
		  <button @click.stop="incDec">↑</button>
		  <button @click.stop="decDec">↓</button>
		</div>

          
		<div class="input-row">
		  <label>視野角</label>
		  <input type="number" min="0" max="720" :step="stepValue" v-model="fieldOfViewDeg" />
		  <button @click.stop="incFov">↑</button>
		  <button @click.stop="decFov">↓</button>
		</div>



		<div class="input-row">
		<label>値ステップ: </label>
		<input type="number" min="0" max="10" step="0.01" v-model.number="stepValue" />
		</div>
   
   		<div class="input-row">
        <button @click="showGrid = !showGrid" @click.stop >
          {{ showGrid ? '赤経・赤緯グリッド線を非表示' : '赤経・赤緯グリッド線を表示' }}
        </button>
        </div>
   
   <div class="time-display">
    <p>現在時刻（日本時間）: {{ currentTime }}<br>
       現在地: {{ currentLocation.name }}<br>
       緯度: {{ currentLocation.lat }}°, 経度: {{ currentLocation.lon }}°</p>
  </div>
      
  
  </div>  
  </div>  
  </div>
  </div>
  </div>   
<!-- デバッグ用レイアウト　ここまで--> 





<!-- 星図レイアウト　　　　　　　-->

 <div class="display-screen01">
    <label>星図 (vol.3.08)</label>
   








  <div class="star-out-container">


    <!--星図 キャンバス -->
    <div class="star-container01" ref="container">
      <canvas class="star-canvas" ref="canvas" ></canvas>
    </div>
    
    
    <!--星図 UI -->
    <div class="star-container02" >
      <div class="ui-panel" >
      
		<div class="input-row">
 		 <label>赤経(RA):</label>
 		 <input type="text" v-model="raTime" placeholder="12:00:00" />
		</div>

		<div class="input-row">
 		 <label>赤緯(Dec):</label>
 		 <input type="text" v-model="decDms" placeholder="+00:00:00" />
		</div>
		

		<button type="button" @click="applyRaDec">赤経赤緯を反映(星図)</button><br />
	



		<div class="input-row">
		  <label>方位(Az):</label>
		<input type="text" v-model="canvasAz" placeholder="00:00:00" readonly />
		</div>

		<div class="input-row">
  		<label>高度(Alt):</label>
  		<input type="text" v-model="canvasAlt" placeholder="+00:00:00" readonly />
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
  
  
    <!--星図 中段レイアウト-->
  <div class="topUI-out-container">
  
   <div class="topUI-container01" >
      <div class="topUI-panel01" >
      
<button type="button" @click="toggleMagnitudeMenu" @click.stop >等級({{ currentMagnitude }}等級)</button>

<div v-if="showMagnitudeMenu" class="dropdown-menu">
  <ul>
    <li @click="selectMagnitude('05')">5等級まで表示</li>
    <li @click="selectMagnitude('06')">6等級まで表示</li>
    <li @click="selectMagnitude('10')">10等級まで表示</li>
  </ul>
</div>
      



      </div>
    </div>

   <div class="topUI-container02" >
      <div class="topUI-panel02" >
      <button type="button" @click="applyLeading" >赤経赤緯を反映(観測＆星図)</button>
      </div>
    </div>
   </div>
  <!--星図 中段レイアウト ここまで-->
  
  
  
  
  
  
  
  
  <!--星図 下段レイアウト -->
  <div class="underUI-out-container">
    
   <div class="underUI-container01" >
   
   
     <div class="underUI-panel01" >
            
      <!-- ▼スコープ切替ボタン -->
      
      
      <button @click="toggleScopeMenu" @click.stop >スコープ({{ currentScope }}°)</button>
      
        <!-- スコープメニュー -->
        
  		<div v-if="showScopeMenu" class="dropdown-menu">
    		<ul>
      		 <li @click="selectScope(0.05)">メインスコープ（視野角0.05°）</li>
      		 <li @click="selectScope(1)">サブスコープ（視野角1°）</li>
      		 <li @click="selectScope(30)">広角スコープ（視野角30°）</li>
    		</ul>
  		</div>
	      
      <button type="button" @click.stop >星図切替</button>
      
     </div>
      
      
      
      
    </div>

   <div class="underUI-container02" >
      <div class="underUI-panel02">
      
      
      <input type="text" v-model="starname" placeholder="天体の名前" readonly />
      
 
	<!-- ▼ 太陽系天体 -->
	<button @click="toggleSolarSystemMenu" @click.stop >太陽系</button>

	<div v-if="showSolarSystemMenu" class="dropdown-menu" >
	  <ul>
	    <li
	      v-for="solarSystemObj in solarSystemObjects"
	      :key="solarSystemObj.name"
	      @click="selectSolarSystemObject(solarSystemObj)"
	    >
	      {{ solarSystemObj.name }}
	    </li>
	  </ul>
	</div>


	<!-- ▼ 恒星 -->
	<button @click="toggleStarMenu" @click.stop >恒星</button>

	<div v-if="showStarMenu" class="dropdown-menu">
	  <ul>
	    <li
	      v-for="fixed in fixedStars"
	      :key="fixed.name"
	      @click="selectFixedStar(fixed)"
	    >
	      {{ fixed.name }}
	    </li>
	  </ul>
	</div>


	<!-- ▼ 非恒星状天体 -->
	<button @click="toggleMenu" @click.stop >非恒星状天体</button>

	<div v-if="showMenu" class="dropdown-menu">
	  <ul>
	    <li
	      v-for="obj in nonStellarObjects"
	      :key="obj.name"
	      @click="selectObject(obj)"
	    >
	      {{ obj.name }}
	    </li>
	  </ul>
	</div>

      
      
      

      </div>
    </div>
    
    
    
    
    
    
   </div>
  

      
  <!--星図 下段レイアウト ここまで-->
   
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
      
      <button type="button" @click="logout">ログアウト</button>
      
      <label>残り時間(秒):</label>
      <input type="text" v-model="logcount" placeholder="1800" readonly />
      </div>
      
      
    </div>

   <div class="statusUI-container02" >
   
      <div class="statusUI-panel02" >
 
 	  <textarea v-model="statuslog" placeholder=">0 test log" readonly></textarea> 
 	  
 	  <!--
      <input type="text" v-model="statuslog" placeholder="1>送信成功" readonly />
		-->
      </div>
    </div>
        
  <!-- テスト用ボタン -->
  
  <!--
  <button @click="addLog('add',2)">ad</button>
  -->
        
    
   </div>
      		
   
  
  
  
 </div>

<!-- ステータス用レイアウト　ここまで--> 




<!-- デバッグ用レイアウト--> 

	<button @click="showTestUI = !showTestUI">
	  {{ showTestUI ? 'テストUIを隠す' : 'テストUIを表示' }}
	</button>
	
<!-- デバッグ用レイアウト　ここまで--> 





<!--導入機能 高度判定メッセージ -->  
<div v-if="showAltError" class="popup">
  {{ altErrorMessage }}
  <button @click="showAltError = false">OK</button>
</div> 

 

<!-- プルダウンメニュー　キャンセルクリック検知用 ここまで-->


 
</template>

















<script setup>
import { ref, watch, onMounted, onUnmounted,onBeforeUnmount ,nextTick ,computed } from 'vue'

//太陽系天体用のライブラリ
import * as Astronomy from "astronomy-engine";

//デバッグ用UIの表示非表示
const showTestUI = ref(false) // false = 非表示、true = 表示



// ===== デバッグ　関連 =====

function incRA() {
  centerRA.value += stepValue.value
  if (centerRA.value >= 360) centerRA.value -= 360
  
  //変更した中心RAをテキストボックスの値に反映させる
  raTime.value = degToHms(centerRA.value)
  
  drawStars()
}

function decRA() {
  centerRA.value -= stepValue.value
  if (centerRA.value < 0) centerRA.value += 360
  
  //変更した中心RAをテキストボックスの値に反映させる
  raTime.value = degToHms(centerRA.value)

  drawStars()
}


function incDec() {
  centerDec.value += stepValue.value
  if (centerDec.value > 90) centerDec.value = 90
  
  //変更した中心DECをテキストボックスの値に反映させる
  //decDms.value = degToHms(centerDec.value)
  decDms.value = degToDms(centerDec.value)
  
  drawStars()
}

function decDec() {
  centerDec.value -= stepValue.value
  if (centerDec.value < -90) centerDec.value = -90
  
  //変更した中心DECをテキストボックスの値に反映させる
  //decDms.value = degToHms(centerDec.value)
  decDms.value = degToDms(centerDec.value)

  
  drawStars()
}


function incFov() {
  fieldOfViewDeg.value += stepValue.value
  drawStars()
}

function decFov() {
  fieldOfViewDeg.value -= stepValue.value
  if (fieldOfViewDeg.value < 1) fieldOfViewDeg.value = 1
  drawStars()
}





//プルダウンメニューを全部閉じる関数
function closeAllMenus() {
  showSolarSystemMenu.value = false
  showStarMenu.value = false
  showMenu.value = false

  showGainMenu.value = false
  showExposureMenu.value = false
  
  showMagnitudeMenu.value = false
  
  showScopeMenu.value = false
  showScopeMenu02.value = false
}



// ===== ログ　関連 =====
const statuslog = ref('>0 test log')         // ログ本体　初期値指定はテンプレートと同じにする
const logarea = ref(null)         // textarea の参照

// ログ左側の数　カウンタ（1スタート）
const logCounter = ref(1)

function addLog(...items) {
  // 引数を全部まとめて文字列化（undefined → ""）
  const text = items.map(v => v ?? "").join("")

  // ログ行を組み立て
  const line = ">" + logCounter.value + " " + text

  // カウンタを次に備えて＋1
  logCounter.value++

  // 最新を上に追加
  statuslog.value = line + "\n" + statuslog.value

  // スクロールをトップへ
  nextTick(() => {
    if (logarea.value) {
      logarea.value.scrollTop = 0
    }
  })
}


// ===== ログイン　関連　URLパラメータ取得 =====
const params = new URLSearchParams(window.location.search)

const urlParamMode = params.get('mode')
const urlParamSite = params.get('site')
const urlParamLang = params.get('lang')
const urlParamDate = params.get('date')



// ===== ログアウト　関連 =====

//ログアウトまでのカウンタ
const logcount = ref(1800) //デフォルト値1800秒

//タイマー処理の管理用
let timerId = null

function logout() {

  //window.location.href = "https://www.kitp.org/itp/telescope/html/start.html?lang=ja_JP"
  //window.location.href = "https://graphicsquare-kt.onrender.com/login.html"
  window.location.href = "../../../login.html"

}




// ===== 観測画面　感度　露出時間　関連 =====



// メニュー表示状態
const showGainMenu = ref(false)   //感度
const showExposureMenu = ref(false) //露出時間


// メニュー開閉 
const toggleGainMenu = () => {

  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showGainMenu.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showGainMenu.value = !isOpen


  //showGainMenu.value = !showGainMenu.value
}

const toggleExposureMenu = () => {


  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showExposureMenu.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showExposureMenu.value = !isOpen




  //showExposureMenu.value = !showExposureMenu.value
}



//ボタン上の見た目の値
const lookButtonGain = ref(10)  // ボタン表示用変数　初期値 10
const lookButtonExposure = ref(9)  // ボタン表示用変数　初期値 9


//観測画面 感度　選択時
const selectGainMenu = (valueGain) => {

  
  lookButtonGain.value = valueGain   // ←ここでボタン表示用変数を更新


  //内部処理用の感度を変更
  gain.value = valueGain


  showGainMenu.value = false //プルダウンメニューを閉じる
  
  //視野を変更したら観測画面を再描画
  drawCanvasUnit()
  
  //ログ
  addLog('観測 感度を変更:', valueGain )
}



//観測画面　露出時間　選択時
const selectExposureMenu = (valueExposure) => {

  lookButtonExposure.value = valueExposure   // ←ここでボタン表示用変数を更新


  //内部処理用の露出時間を変更
  exposure.value = valueExposure


  showExposureMenu.value = false //プルダウンメニューを閉じる
  

  //視野を変更したら観測画面を再描画
  drawCanvasUnit()

  //ログ
  addLog('観測 露出時間を変更:', valueExposure)
}










//実際に内部処理で使う値　※ボタンの見た目↑の初期値と整合させること！！
const gain = ref(10)    // 0〜10　感度
const exposure = ref(9) // 0〜9　露出時間


// 明るさを合算して算出（0〜19）
const brightness = computed(() => exposure.value + gain.value)


// 非恒星状天体用 明るさ係数　0～1の割合で明るさを制御
function getNebulaBrightnessFactor(brightness) {
  if (brightness <= 14) return 0
  return (brightness - 14) / (19 - 14) // 0〜1
}



// ===== 観測画面　天体画像関連 =====


// canvas参照
const pictureCanvas = ref(null)
const ctx_pic = ref(null)


// 表示する画像
let starImg = new Image()


// 中心座標をもとに画像ファイル名を生成
function getImageFileName(pic_raDeg, pic_decDeg) {
  const XXX = Math.round(pic_raDeg)
  const YYY = Math.round(pic_decDeg + 90)
  return `./picture/sm_r${XXX}d${YYY}.png`
}

//ファイル名からRA　DECを取得
function parseImageCenter(fileName) {
  const match = fileName.match(/r(\d+)d(\d+)/)
  if (!match) return null
  return {
    ra: Number(match[1]),
    dec: Number(match[2]) - 90
  }
}





// 観測画面　天体画像を描画
function drawCanvas(centerRaDeg, centerDecDeg) {
  const canvas = pictureCanvas.value
  if (!canvas) return

  //ctx_pic.value = canvas.getContext('2d')
  //効率化・高速化するらしい？
  ctx_pic.value = canvas.getContext('2d', { willReadFrequently: true })


  // ← ここでキャンバスのサイズを固定
  canvas.width = 640
  canvas.height = 480
  canvas.style.width = '100%'
  canvas.style.height = 'auto'




  // 画像ファイル名
  const imgFile = getImageFileName(centerRaDeg, centerDecDeg)
  
  
  //下の処理に移動させた　（最後にした方が良いらしい）
  //starImg.src = imgFile

  // ファイル名から画像中心RA/DECを取得
  const imgCenter = parseImageCenter(imgFile)

  // エラーハンドリング
  starImg.onerror = () => {
  
    //console.warn('画像が見つかりません:', imgFile)
    starImg.src = './picture/none.png'
    
    
  }


  starImg.onload = () => {
  

  
  
  
  
    const imgW = 1280
    const imgH = 960

    const degPerPixelX = 2 / imgW
    const degPerPixelY = 2 / imgH

    const viewPxW = viewSize.value / degPerPixelX
    const viewPxH = viewSize.value / degPerPixelY

    //  ファイル名から取得した値を使う
    const imgCenterRa = imgCenter.ra
    const imgCenterDec = imgCenter.dec

    // RA/DECオフセット
    let raOffsetDeg = centerRaDeg - imgCenterRa
    if (raOffsetDeg > 180) raOffsetDeg -= 360
    if (raOffsetDeg < -180) raOffsetDeg += 360

    const decOffsetDeg = centerDecDeg - imgCenterDec

    // ピクセルオフセット（RAは反転）
    const offsetX = -raOffsetDeg / degPerPixelX
    const offsetY = -decOffsetDeg / degPerPixelY

    const sx = imgW / 2 - viewPxW / 2 + offsetX
    const sy = imgH / 2 - viewPxH / 2 + offsetY



 // ===== ここから追加 =====
  const factor = getNebulaBrightnessFactor(brightness.value)


//画面の初期化
  ctx_pic.value.clearRect(0, 0, canvas.width, canvas.height)

  // 完全に暗い場合は何も描かない
  if (factor <= 0) return

  ctx_pic.value.save()
  ctx_pic.value.globalAlpha = factor



    ctx_pic.value.drawImage(starImg, sx, sy, viewPxW, viewPxH, 0, 0, canvas.width, canvas.height)
  ctx_pic.value.restore()
   }


	// onload / onerror を設定したあとに、画像読み込みを開始する
	
	// 最後に src をセットする
	starImg.src = imgFile

    

}



// RA/DEC文字列を度数に変換（RA: "21:32:30" → 323.125度）
function raStrToDeg(raStr) {
  const [h, m, s] = raStr.split(':').map(Number)
  return (h + m/60 + s/3600) * 15
}


// DEC文字列を度数に変換（Dec: "-01:00:30" → -1.0083度）
function decStrToDeg(decStr) {
  const sign = decStr.startsWith('-') ? -1 : 1
  const [d, m, s] = decStr.replace('+','').replace('-','').split(':').map(Number)
  return sign * (d + m/60 + s/3600)
}




// ===== 高度判定処理の管理フラグ　関連 =====

//高度０以下のとき処理停止の管理用フラグ
//true→「高度０なら処理を中断」の判定をしない＝どんなときも表示
//false→「高度０なら処理を中断」の判定を行う
const skipFlagAltJudgePictureCanvas = ref(false) // 画像キャンバスのフラグ false=処理停止判定を行う（スキップしない）
const skipFlagAltJudgeStarCanvas = ref(true)    // 星図キャンバスのフラグ true=処理停止判定を行わない（スキップする）
const canvasAltDeg = ref(0)//高度判定用


//フラグ管理効率化　高度判定処理
function canDrawByAlt(altDeg, skipFlag, logMessage) {

  // 判定をスキップするなら、無条件で描画OK
  if (skipFlag) return true

  // 高度が0未満なら描画NG＋ログ
  if (altDeg < 0) {
    if (logMessage) {
      addLog(logMessage)
      
	  //自作functionによる　アナウンス
      //showAltErrorPopup('高度が0度未満のため、更新できません')
      showAltErrorPopup(logMessage)
      
      //アラートによる強制停止　強めの警告
      //alert("高度が低すぎるため導入を中止しました")

    }
    return false
  }

  return true
}


//高度判定ポップアップ用
const showAltError = ref(false)
const altErrorMessage = ref('')

let altErrorTimer = null

//エラーのポップアップ用
function showAltErrorPopup(message) {
  altErrorMessage.value = message
  showAltError.value = true
}



// ===== 観測画面　画像保存JPG　関連 =====


//画像の保存

const saveAsJpg = () => {
  if (!pictureCanvas.value) return
  if (fileNameRaDeg.value === null) return


  const fileName = createFileName(
    fileNameRaDeg.value,
    fileNameDecDeg.value
  )


  //文字列データ に変換 引数（画像形式、画質1=100%）
  const dataUrl = pictureCanvas.value.toDataURL('image/jpeg', 1.0)

  const link = document.createElement('a')
  link.href = dataUrl
  
  //link.download = 'star-chart.jpg'
  link.download = fileName
  
  link.click()
}


//ファイル名作成用
const formatNumber = (value) => {

  //正の数　負の数　判定して文字をつける
  //const sign = value >= 0 ? 'plus' : 'minus'
  const sign = value >= 0 ? '' : ''
  
  const abs = Math.abs(value).toFixed(1)
  return sign + abs.replace('.', '_')
}


const createFileName = (ra, dec) => {

  //const now = new Date()
  const now = getFixedUTCDate()

  const yyyy = now.getUTCFullYear()
  const mm = String(now.getUTCMonth() + 1).padStart(2, '0')
  const dd = String(now.getUTCDate()).padStart(2, '0')
  const hh = String(now.getUTCHours()).padStart(2, '0')
  const mi = String(now.getUTCMinutes()).padStart(2, '0')
  const ss = String(now.getUTCSeconds()).padStart(2, '0')

  const raStr = formatNumber(ra)
  const decStr = formatNumber(dec)

  return `${yyyy}${mm}${dd}_${hh}${mi}${ss}_RA_${raStr}_DEC_${decStr}.jpg`
}


//ファイル名用の変数
const fileNameRaDeg = ref(null)
const fileNameDecDeg = ref(null)


//観測画面　描画　再反映用（観測画面のみの反映　の機能がなかったので後付けで作った）
function drawCanvasUnit() {

  const pic_raDeg = raStrToDeg(raTime.value)
  const pic_decDeg = decStrToDeg(decDms.value)
  
  //観測画面 描画の反映
  drawCanvas(pic_raDeg, pic_decDeg)


  //ここからファイル名用の処理
  
   // DECを +90 して正の数にする
   const decShifted = pic_decDeg + 90

   // ファイル名用変数の更新
   fileNameRaDeg.value = pic_raDeg
   fileNameDecDeg.value = decShifted


//度数表記を確認するログ
//addLog('観測の描画更新:','RA(deg):', pic_raDeg.toFixed(1) ,' Dec(deg):', pic_decDeg.toFixed(1) )

}


// 導入ボタン　２つのキャンバスへ赤経赤緯の反映
function applyLeading() {


  // 地平線下なら画像キャンバスを止める（＆スキップフラグの分岐処理）
  if (canDrawByAlt(
  canvasAltDeg.value, 
  skipFlagAltJudgePictureCanvas.value,
   "観測の描画を中止しました（高度が0より小さいため）"
  )
  ) {

  //観測画面 描画の反映
  drawCanvasUnit()

  
  //観測画面 テキストボックス「値」の反映処理
  applySlideValue()
  
  }
  
  
  
  // 地平線下なら星図キャンバスを止める（＆スキップフラグの分岐処理）  
  if (canDrawByAlt(
  canvasAltDeg.value,
  skipFlagAltJudgeStarCanvas.value,
  "星図の描画を中止しました（高度が0より小さいため）"
  )
  ) {
  
  //導入を押したときは、星図の赤経赤緯の描画もついでにしておく
  applyRaDec()
  
  }
  
}

// 表示範囲の反映
function applyViewRange() {
  applyLeading()
}

// 初期描画
//applyLeading()




























// ===== 星図　天体選択メニュー　関連 =====


//選択した天体名　テキストボックスに表示する用
const starname = ref('')




//太陽系天体メニュー


const showSolarSystemMenu = ref(false)

function toggleSolarSystemMenu() {

  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showSolarSystemMenu.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showSolarSystemMenu.value = !isOpen
}




const solarSystemObjects = ref([
  { name: '太陽', body: Astronomy.Body.Sun },
  { name: '月',   body: Astronomy.Body.Moon },
  { name: '水星', body: Astronomy.Body.Mercury },
  { name: '金星', body: Astronomy.Body.Venus },
  { name: '火星', body: Astronomy.Body.Mars },
  { name: '木星', body: Astronomy.Body.Jupiter },
  { name: '土星', body: Astronomy.Body.Saturn },
  { name: '天王星', body: Astronomy.Body.Uranus },
  { name: '海王星', body: Astronomy.Body.Neptune },
])


//選択された瞬間に RA / Dec を計算する
function selectSolarSystemObject(obj) {

  // 観測日時（すでに作ってあるもの流用）
  const date = getFixedUTCDate()

  // 観測地（緯度・経度）
  const observer = new Astronomy.Observer(
    currentLocation.value.lat,
    currentLocation.value.lon,
    0
  )

  // 赤道座標（J2000ではなく date 時点）
  const equ = Astronomy.Equator(
    obj.body,
    date,
    observer,
    true,   // ofDate
    true    // aberration
  )

  // 数値 → 00:00:00 形式に変換
  raTime.value = degToHms(equ.ra * 15)   // hours → deg
  decDms.value = degToDms(equ.dec)

  starname.value = obj.name
  
  showSolarSystemMenu.value = false
 
  // 画面反映
  applyRaDec()
}




//恒星メニュー


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


//プルダウンメニューの開閉用
function toggleStarMenu() {


  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showStarMenu.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showStarMenu.value = !isOpen



  //showStarMenu.value = !showStarMenu.value
  //showSolarSystemMenu.value = false
  //showMenu.value = false


}



//恒星選択時の処理
function selectFixedStar(fixed) {
  raTime.value = fixed.ra
  decDms.value = fixed.dec
  starname.value = fixed.name
  showStarMenu.value = false
  
  //赤経赤緯の画面への反映処理（ボタンを押下と一緒）
  applyRaDec()
}



//非恒星状天体



// 非恒星状天体　メニュー表示フラグ
const showMenu = ref(false)

// 非恒星状天体リスト（例）
const nonStellarObjects = ref([


  { name: 'M1（かに星雲）', ra: '05:34:30', dec: '+22:01:00' },
  { name: 'M2', ra: '21:33:30', dec: '-00:49:00' },
  { name: 'M3', ra: '13:42:12', dec: '+28:23:00' },
  { name: 'M4', ra: '16:23:36', dec: '-26:32:00' },
  { name: 'M5', ra: '15:18:36', dec: '+02:05:00' },
  { name: 'M6（バタフライ星団）', ra: '17:40:06', dec: '-32:13:00' },
  { name: 'M7', ra: '17:53:54', dec: '-34:49:00' },
  { name: 'M8（干潟星雲）', ra: '18:03:48', dec: '-24:23:00' },
  { name: 'M9', ra: '17:19:12', dec: '-18:31:00' },
  { name: 'M10', ra: '16:57:06', dec: '-04:06:00' },
  { name: 'M11', ra: '18:51:06', dec: '-06:16:00' },
  { name: 'M12', ra: '16:47:12', dec: '-01:57:00' },
  { name: 'M13（ヘラクレス球状星団）', ra: '16:41:42', dec: '+36:28:00' },
  { name: 'M14', ra: '17:37:36', dec: '-03:15:00' },
  { name: 'M15', ra: '21:29:12', dec: '+12:10:00' },
  { name: 'M16（わし星雲）', ra: '18:18:48', dec: '-13:48:00' },
  { name: 'M17（オメガ星雲）', ra: '18:20:26', dec: '-16:10:00' },
  { name: 'M18', ra: '18:21:36', dec: '-12:18:00' },
  { name: 'M19', ra: '17:02:36', dec: '-26:16:00' },
  { name: 'M20（三裂（トリプル）星雲）', ra: '18:02:42', dec: '-23:01:00' },
  { name: 'M21', ra: '18:04:24', dec: '-22:28:00' },
  { name: 'M22', ra: '18:36:24', dec: '-23:54:00' },
  { name: 'M23', ra: '17:57:12', dec: '-19:02:00' },
  { name: 'M24（スタークラウド）', ra: '18:18:24', dec: '-18:25:00' },
  { name: 'M25', ra: '18:31:54', dec: '-19:06:00' },
  { name: 'M26', ra: '18:45:36', dec: '-09:24:00' },
  { name: 'M27（あれい状星雲）', ra: '19:59:36', dec: '+22:43:16' },
  { name: 'M28', ra: '18:24:32', dec: '-24:52:00' },
  { name: 'M29', ra: '20:23:12', dec: '+38:38:00' },
  { name: 'M30', ra: '21:40:48', dec: '-23:12:00' },
  { name: 'M31（三角座銀河／アンドロメダ銀河）', ra: '00:42:44', dec: '+41:16:09' },
  { name: 'M32', ra: '00:42:54', dec: '+40:52:00' },
  { name: 'M33（三角座銀河）', ra: '01:33:50', dec: '+30:39:37' },
  { name: 'M34', ra: '02:42:00', dec: '+42:47:00' },
  
  
  { name: 'M42（オリオン大星雲）', ra: '05:35:17', dec: '-05:23:28' },
  { name: 'M44（プレセペ星団）', ra: '08:40:24', dec: '+19:59:00' },
  { name: 'M45（プレアデス星団）', ra: '03:47:00', dec: '+24:07:00' },
  { name: 'M57（こと座リング星雲）', ra: '18:53:35', dec: '+33:01:45' },
  { name: 'M51（子持ち銀河）', ra: '13:29:52', dec: '+47:11:43' },
  { name: 'M64（黒眼銀河）', ra: '12:56:43', dec: '+21:41:00' },
  
  { name: 'M101（回転花火銀河）', ra: '14:03:13', dec: '+54:20:57' },
  { name: 'M104（ソンブレロ銀河）', ra: '12:39:59', dec: '-11:37:23' },
  { name: 'M104（ソンブレロ銀河）', ra: '12:39:59', dec: '-11:37:23' },
  
  { name: 'NGC 104（47トゥカーナ星団）', ra: '00:24:05', dec: '-72:04:52' },
  { name: 'NGC 2237（ばら星雲）', ra: '06:31:55', dec: '+04:56:34' },
  { name: 'NGC 253（ちょうこくしつ座銀河）', ra: '00:47:33', dec: '-25:17:18' },
  { name: 'NGC 3115（紡錘銀河）', ra: '10:05:14', dec: '-07:43:06' },
  { name: 'NGC 3372（カリーナ星雲）', ra: '10:45:08', dec: '-59:52:40' },
  { name: 'NGC 5128（ケンタウルスA銀河）', ra: '13:25:28', dec: '-43:01:09' },
  { name: 'NGC 5139（オメガ星団）', ra: '13:26:47', dec: '-47:28:46' },
  { name: 'NGC 7000（北アメリカ星雲）', ra: '20:58:54', dec: '+44:19:00' },
  { name: 'NGC 7293（らせん星雲）', ra: '22:29:38', dec: '-20:50:13' },

  { name: 'SMC（小マゼラン雲）', ra: '00:52:38', dec: '-72:48:01' },
  { name: 'IC 2602（南のプレアデス）', ra: '10:42:57', dec: '-64:24:00' },
  { name: 'LMC（大マゼラン雲）', ra: '05:23:34', dec: '-69:45:22' }

])






// 非恒星状天体　プルダウンメニューの開閉
function toggleMenu() {


  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showMenu.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showMenu.value = !isOpen



  //showMenu.value = !showMenu.value
  
  //showSolarSystemMenu.value = false
  //showStarMenu.value = false
}




// 非恒星状天体を選択したときの処理
function selectObject(obj) {
  raTime.value = obj.ra
  decDms.value = obj.dec
  showMenu.value = false  // メニューを閉じる
  starname.value = obj.name  // 天体名の反映
  
  
  //赤経赤緯の画面への反映処理（ボタンを押下と一緒）
  applyRaDec()
  
}





// ===== 星図の等級　関連 =====

const currentMagnitude = ref(5)

//ファイル指定　初期は５等級　※必ずtemplateのUI（ボタンの見た目）とそろえること
const selectedFile = ref('./star-data_05.json')

//プルダウンメニューの表示ONOFF管理
const showMagnitudeMenu = ref(false)

//開閉スイッチ　プルダウンメニューが開いてたら閉じる　閉じてたら開く　両パターン対応

function toggleMagnitudeMenu() {


  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showMagnitudeMenu.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showMagnitudeMenu.value = !isOpen


  //showMagnitudeMenu.value = !showMagnitudeMenu.value



}

//ファイルの切り替え処理　＆　再描画
function selectMagnitude(level) {
  selectedFile.value = `./star-data_${level}.json`
  
  currentMagnitude.value = Number(level)   // ←これだけ追加！
  
  loadStarData()
  showMagnitudeMenu.value = false
  
  //ログ
  addLog('表示等級の変更:',currentMagnitude.value ,'等級まで')
}




// ===== 星図のキャンバスの基本設定　関連 =====


// キャンバス比率を固定
const aspectRatio = 4 / 3  

//星図のキャンバス
const canvas = ref(null)

//コンテナ（HTML的な外枠としての役割）
const container = ref(null)

//星図の方位と高度
const canvasAz = ref('')  // 方位（Az）
const canvasAlt = ref('')  // 高度（Alt）

//観測の方位と高度
const canvasAz_Slide = ref('')  // 方位（Az）
const canvasAlt_Slide = ref('')  // 高度（Alt）




const hour = ref(0)

const stars = ref([])

// 初期値（12h = 180°）
//const raHour = ref(12) 







// === 赤経赤緯 関連 ===


//星図の赤経赤緯の値

const raTime = ref("12:00:00") // 初期表示
const decDms = ref("+00:00:00") // 表示値（度分秒）

// 観測の赤経赤緯
const raTime_Slide = ref("00:00:00") // 初期表示
const decDms_Slide = ref("+00:00:00") // 表示値（度分秒）


//星図の描画　表示の中心とする赤経赤緯座標

//※必ず、星図の赤経赤緯の値の初期値と合わせる！！　初期表示がズレる
const centerRA = ref(180)       // 内部的には度数で管理（初期値12hに合わせて 180°）
const centerDec = ref(0) // 内部的には度



//星図の赤経赤緯、方位高度のテキストボックスの「値」を観測画面に反映
function applySlideValue() {
  raTime_Slide.value   = raTime.value
  decDms_Slide.value   = decDms.value
  canvasAz_Slide.value = canvasAz.value
  canvasAlt_Slide.value = canvasAlt.value
  
  //ログ
  //addLog('観測の描画更新:','RA(deg):', raTime_Slide.value ,' Dec(deg):', decDms_Slide.value )
  addLog('観測の描画更新:成功')


}




// ===== 視野角　関連 =====
const currentScope = ref(1)  // ボタン表示用変数　初期値 1°
const currentScope02 = ref(1)  // ボタン表示用変数　初期値 1°

//描画用の視野
const fieldOfViewDeg = ref(1) // 星図用 計算や表示に利用する　視野角（初期値 1）
const viewSize = ref(1.0) // 観測用 計算や表示に利用する　視野角（初期値 1）

// メニュー表示状態
const showScopeMenu = ref(false)
const showScopeMenu02 = ref(false)


// メニュー開閉 星図
const toggleScopeMenu = () => {

  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showScopeMenu.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showScopeMenu.value = !isOpen


  //showScopeMenu.value = !showScopeMenu.value
}

// メニュー開閉 観測
const toggleScopeMenu02 = () => {

  //プルダウンメニューの現在の状態を一時保管
  const isOpen = showScopeMenu02.value

  //プルダウンメニューを全て閉じる
  closeAllMenus()

  //スイッチする（最初の状態と逆にする）
  showScopeMenu02.value = !isOpen


  //showScopeMenu02.value = !showScopeMenu02.value
}




//星図 スコープ選択時
const selectScope = (deg) => {

    currentScope.value = deg   // ←ここでボタン表示用変数を更新
    
  fieldOfViewDeg.value = deg   // ←ここで画面表示用の値を更新

  showScopeMenu.value = false //プルダウンメニューを閉じる
  

  //視野を変更したら星図画面を再描画する必要あり、しかし
  //watchの監視対象 fieldOfViewDeg　により自動で drawStars() が実行される
  //よって、ここには再描画は実行不要　（後々の効率化で、ここは仕様変更するかも）

  //→watchの廃止をしたため、描画を手動更新
  drawStars()

  //ログ
  addLog('星図 視野角を変更:', deg ,'°')
}



//観測画面 スコープ選択時
const selectScope02 = (deg) => {

    currentScope02.value = deg   // ←ここでボタン表示用変数を更新
    
  viewSize.value = deg   // ←ここで画面表示用の値を更新

  showScopeMenu02.value = false //プルダウンメニューを閉じる
  
  //視野を変更したら観測画面を再描画
  drawCanvasUnit()
  
  //ログ
  addLog('観測 視野角を変更:', deg ,'°')
}




// UI操作関連
const stepValue = ref(1) // ステップ（初期値）
const showGrid = ref(false)






// =====星図の描画　関連 =====




//色の反映設定
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




//グリッド線　赤経赤緯
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


// ▼ ラベル（RA）を赤緯0度（天の赤道）に表示
const labelDec = 0
const labelPos = raDecToCanvasXY(
  ra,
  labelDec,
  width,
  height,
  centerRA.value,
  centerDec.value,
  fieldOfViewDeg.value
)

// 画面内にあるときだけ描画
if (
  labelPos.x >= 0 && labelPos.x <= width &&
  labelPos.y >= 0 && labelPos.y <= height
) {
  const raHours = ((ra / 15) + 24) % 24
  ctx.textAlign = 'center'
  ctx.textBaseline = 'top'
  ctx.fillText(`${raHours}h`, labelPos.x, labelPos.y + 4)
}



    // ラベル（RA）を一番上の点に表示（RAは時間単位に変換）
    //if (firstXY) {
    //  const raHours = ((ra / 15) + 24) % 24 // 赤経を0〜23hで表示
    //  ctx.fillText(`${raHours}h`, firstXY.x, 5)
    // }
    
    
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








//星図画面　星の描画
function drawStars() {
  //const ctx = canvas.value.getContext('2d')
  //高速化するらしい
  const ctx = canvas.value.getContext('2d',{ willReadFrequently: true })
  
  const width = canvas.value.width
  const height = canvas.value.height

  //画面の初期化　黒背景で塗りつぶし＆fillのクリア
  ctx.fillStyle = 'black'
  ctx.fillRect(0, 0, width, height)

  //変換　時間表記を度数に
  const rotationDeg = hour.value * 15
  const rotationRad = (rotationDeg / 360) * 2 * Math.PI



  // グリッドの描画（表示ONの場合のみ）
  if (showGrid.value) {
    drawGrid(ctx, width, height)
  }



  // jsonデータからの描画＝恒星の描画
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


	const rawScale = 1 / fieldOfViewDeg.value

	let baseSize = 1.4
	let scaleFactor = 0.20
	let minSize = 1
	let fovScale = Math.max(1, Math.sqrt(rawScale))
	
	if (fieldOfViewDeg.value === 30) {

		baseSize = 1.4
		scaleFactor = 0.20
		minSize = 1
		fovScale = 1.0

		}
		
		
	else if (fieldOfViewDeg.value === 1) {
		baseSize = 2.0
		scaleFactor = 0.30
		minSize = 2
		fovScale = 1.8
		}
		
	else if (fieldOfViewDeg.value === 0.05) {
		baseSize = 2.8
		scaleFactor = 0.35
		minSize = 5
		fovScale = 2.6
		}

	//有名恒星など明るい星ほど大きく
	let brightBoost = 1.0
		if (mag < 2.5) brightBoost = 1.5
		if (mag < 2.0) brightBoost = 1.75
		if (mag < 1.0) brightBoost = 2
		if (mag < 0.0) brightBoost = 2.5


	// 星のサイズを決定
     //const baseSize = 1.5
     //const scaleFactor = 0.3

	//星の最小サイズ　0.5
	//const minSize = 0.4
	//const minSize = 0.5

	//スコープによって条件分岐させる
     //const minSize = 1

	//描画する星の最大サイズ
    //const maxSize = 4.0
    const maxSize = 100.0  // ← 少し上限を広げる　10～50　上限はなくてもよいかも
    
    
    //サイズの視野基準　1～60
    //const rawScale = 40 / fieldOfViewDeg.value
    //const rawScale = 10 / fieldOfViewDeg.value
    
    
	 //const fovScale = Math.max(1, Math.sqrt(rawScale))
	//const radius = Math.max(minSize, Math.min(maxSize, (baseSize - scaleFactor * mag) * fovScale))


	
	//星の描画半径の式
	const radius = Math.max(minSize, Math.min(maxSize, (baseSize - scaleFactor * mag) * fovScale * brightBoost))


    
    // ★追加：視野角による拡大補正（視野60°を基準）
	// const fovScale = Math.max(1, 60 / fieldOfViewDeg.value)  
	// 例：視野60°なら倍率1倍、視野6°なら10倍、0.6°なら100倍など
    //const radius = Math.max(minSize, Math.min(maxSize, (baseSize - scaleFactor * mag) * fovScale))
    //const radius = Math.max(minSize, Math.min(maxSize, baseSize - scaleFactor * mag))
    
    
    
    
    //色
    const color = getStarColor(bv)
    
    
    
    
    //透明度
    //const alpha = Math.min(1.0, Math.max(0.3, 1.5 - mag / 2))
    //const alpha = Math.min(1.0, Math.max(0.7, 1.2 - mag / 5))
	//const alpha = Math.min(1.0, Math.max(1, 1.2 - mag / 5))
    //const alpha = Math.min(1.0,Math.max(0.7, 1.1 - mag / 8))
    
    const alpha = Math.min(1.0,Math.max(0.7, 1.0 - (mag - 0) * 0.05 ))
    
   



    ctx.beginPath()
    ctx.arc(x, y, radius, 0, Math.PI * 2)
    ctx.fillStyle = hexToRgba(color, alpha)
    ctx.fill()
  })
  //forEachここまで
  
  //明るさ調整の関数
  //とりあえず、今は不要。視野ごとにスケールを分ける機能を追加したので↑
  //applyCanvasBrightnessPreserveColor(ctx, width, height, 20.0)
  
  
  
  
	//太陽系天体描画の実行
	drawSolarSystem(ctx)


}
//drawStars()ここまで












//hex=16進数　色とアルファ透明度を変換する
function hexToRgba(hex, alpha = 1) {

 //parseInt 文字型を数値型に変換　　slice 文字を指定位置から取り出し
 //parseInt(,16)  16進数扱い
  const r = parseInt(hex.slice(1, 3), 16)
  const g = parseInt(hex.slice(3, 5), 16)
  const b = parseInt(hex.slice(5, 7), 16)

  return `rgba(${r}, ${g}, ${b}, ${alpha})`
}




//星図　キャンバスがリサイズしたときの描画処理
function resizeCanvas() {
  if (!canvas.value || !container.value) return

  const parentWidth = container.value.clientWidth
  const newWidth = parentWidth
  const newHeight = parentWidth / aspectRatio  // ← 比率を維持

  canvas.value.width = newWidth
  canvas.value.height = newHeight

  drawStars()
}















//天体データの読み込み
async function loadStarData() {
  try {
    const response = await fetch(selectedFile.value)
    const data = await response.json()

    stars.value = data.filter(star => parseFloat(star.vmag) <= 10.0)

    resizeCanvas()
  } catch (e) {
    console.error('星データの読み込みに失敗しました', e)
  }
}




onBeforeUnmount(() => {
  window.removeEventListener('resize', resizeCanvas)
})




onMounted(() => {
  
  
  //ログ用の初期設定
  init()
  
  //天体データ読み込み＆リサイズ調整＆再描画
  loadStarData()
  window.addEventListener('resize', resizeCanvas)
})



/* ========== 太陽系天体　用 ========== */


//Astronomy Engine 用の Observer を作る
function getAstronomyObserver() {
  return new Astronomy.Observer(
    currentLocation.value.lat,
    currentLocation.value.lon,
    0 // 高度（とりあえず0mでOK）
  )
}


//太陽系天体の RA / Dec を取得する共通関数
function getSolarRaDec(body) {
  const date = getFixedUTCDate()
  const observer = getAstronomyObserver()

  const eq = Astronomy.Equator(
    body,
    date,
    observer,
    true,  // 観測者基準
    true   // 光行差など補正あり
  )

  return {
    ra: eq.ra * 15, // hour → degree
    dec: eq.dec
  }
}



//太陽系天体　星図に描画（既存関数を使う）
function drawSolarObject(ctx, ra, dec, radius, color) {
  const { x, y } = raDecToCanvasXY(
    ra,
    dec,
    canvas.value.width,
    canvas.value.height,
    centerRA.value,
    centerDec.value,
    fieldOfViewDeg.value
  )

  ctx.beginPath()
  ctx.arc(x, y, radius, 0, Math.PI * 2)
  ctx.fillStyle = color
  ctx.fill()
}


//太陽系天体のそれぞれのパラメータ
const solarSystemBodies = [
  { body: Astronomy.Body.Sun,    name: 'Sun',    color: 'yellow',  baseSize: 10 },
  { body: Astronomy.Body.Moon,   name: 'Moon',   color: '#dddddd', baseSize: 5 },

  { body: Astronomy.Body.Mercury,name: 'Mercury',color: '#bbbbbb', baseSize: 3 },
  { body: Astronomy.Body.Venus,  name: 'Venus',  color: '#fff5cc', baseSize: 4 },
  { body: Astronomy.Body.Mars,   name: 'Mars',   color: 'red',     baseSize: 4 },
  { body: Astronomy.Body.Jupiter,name: 'Jupiter',color: '#ffcc99', baseSize: 5 },
  { body: Astronomy.Body.Saturn, name: 'Saturn', color: '#ffeeaa', baseSize: 5 },

  { body: Astronomy.Body.Uranus,  name: 'Uranus',  color: '#ccffff',  baseSize: 3 },
  { body: Astronomy.Body.Neptune, name: 'Neptune', color: '#99ccff',  baseSize: 3 },
]



//視野角に応じたサイズ補正
function getSolarSize(baseSize) {
  if (fieldOfViewDeg.value <= 0.1) return baseSize * 2.5
  if (fieldOfViewDeg.value <= 1)   return baseSize * 2.0
  if (fieldOfViewDeg.value <= 5)   return baseSize * 1.5
  return baseSize
}




//drawSolarSystem(ctx) 太陽系天体描画の本体
function drawSolarSystem(ctx) {
  const date = getFixedUTCDate()
  
  const observer = new Astronomy.Observer(
    currentLocation.value.lat,
    currentLocation.value.lon,
    0
  )


  solarSystemBodies.forEach(obj => {
  
    const eq = Astronomy.Equator(
      obj.body,
      date,
      observer,
      true,
      true
    )

    const raDeg  = eq.ra * 15
    const decDeg = eq.dec

    drawSolarObject(
      ctx,
      raDeg,
      decDeg,
      getSolarSize(obj.baseSize),
      obj.color
    )
    
  })
  
  
}





/* ========== RA / Dec の変換関数 ========== */


// === （RA）関連 ===
//RA HH:MM[:SS] 文字列を度に変換（RA: 0h - 24h -> 0° - 360°）
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

//RA 度 -> HH:MM:SS 形式（常に 2桁で整形）
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




// === Dec 関連 ===
/* DEC DMS文字列 → 度 に変換 */
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

/* DEC 度 → DMS文字列 に変換 */
function degToDms(deg) {
  const sign = deg >= 0 ? '+' : '-'
  const absDeg = Math.abs(deg)
  const d = Math.floor(absDeg)
  const m = Math.floor((absDeg - d) * 60)
  const s = Math.floor(((absDeg - d) * 60 - m) * 60)
  const pad = (n) => n.toString().padStart(2, "0")
  return `${sign}${pad(d)}:${pad(m)}:${pad(s)}`
}


/* DEC DMS入力時 → 度に更新 */
function updateDecDegFromDms() {
  const deg = dmsToDeg(decDms.value)
  // 範囲チェック（-90〜+90）してから代入
  if (deg > 90) centerDec.value = 90
  else if (deg < -90) centerDec.value = -90
  else centerDec.value = deg
}



//廃止 自動更新するのではなく、しっかりと手動更新する仕様に変更したため

// DEC 度が更新されたら DMS表記も更新
//watch(centerDec, (deg) => {
//  decDms.value = degToDms(deg)
//})


//廃止検討
/* 双方向：RA  中心の degree が変わったら表示文字列を整形 */
//watch(centerRA, (deg) => {
//  raTime.value = degToHms(deg)
//})


//廃止 自動更新するのではなく、しっかりと手動更新する仕様に変更したため

// RA 度 → 時刻 に変換（双方向反映）
//watch(centerRA, (deg) => {
//  const hour = deg / 15
//  const h = Math.floor(hour)
//  const m = Math.floor((hour - h) * 60)
//  const s = Math.floor(((hour - h) * 60 - m) * 60)
//  const pad = (n) => n.toString().padStart(2, "0")
//  raTime.value = `${pad(h)}:${pad(m)}:${pad(s)}`
//})





// ===星図 赤経・赤緯 反映ボタン ===
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

  //ログ
  addLog('星図の描画更新:成功')
  //addLog('星図の描画更新:','RA(deg):', centerRA.value.toFixed(3) ,' Dec(deg):', centerDec.value.toFixed(3) )

  // ▼ 方位・高度の計算
  //const AzAltdate = new Date()       //廃止　現在時刻をもとに方位・高度の計算
  const AzAltdate = getFixedUTCDate()   //廃止→初期化処理に移動 
  
  const result = computeAzAlt(centerRA.value, centerDec.value, currentLocation.value, AzAltdate)
  canvasAz.value = result.az  // 方位
  canvasAlt.value = result.alt // 高度
  
  canvasAltDeg.value = result.altDeg //高度判定用の値
  
  
  //星図の再描画 実行
  drawStars()
  
  
  console.log(
  'RA:', centerRA.value,
  'Dec:', centerDec.value,
  'lat:', currentLocation.value?.lat,
  'lng:', currentLocation.value?.lng,
  'lon:', currentLocation.value?.lon,
  'date:', AzAltdate
  )
  
}

//廃止
//watchで監視 再描画(それぞれのパラメータが変化した瞬間)
//watch([raTime, hour, showGrid, centerRA, centerDec, fieldOfViewDeg], () => {
//  drawStars()
//})


//明るさだけ調整の関数 drawstar()の最後に追加した機能
//色と明るさの調整 両立
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




// ------------------------
// 固定の UTC 時刻（必要なら自由に変えてOK）
// "2021-09-24 00:00:00 UTC" のように UTC を明記
// ------------------------

//日付の手動固定設定
//const fixedUTCString = "2021-09-24 00:00:00"

//URLクエリから取得 書式を合わせるために置き換え　日付直後のハイフンの除去
//const fixedUTCString = urlParamDate.replace(/(\d{4}-\d{2}-\d{2})-/, "$1 ")


//日時入力データ加工 URLにdateがあればそれを、なければ初期値を使う
const fixedUTCString = urlParamDate
  ? urlParamDate.replace(
      /(\d{4}-\d{2}-\d{2})-(\d{2}:\d{2}:\d{2})/,
      "$1T$2Z"
    )
  : "2021-09-24T00:00:00Z"
  
  
//加工された日時のデータを正式な日時形式（Date型）に変換
function getFixedUTCDate() {
  return new Date(fixedUTCString)
}


// 時刻取得（日本時間）
const currentTime = ref('')


// 観測地データ（緯度・経度）
const locations = {
  japan: { name: 'Tokyo, Japan', lat: 35.6895, lon: 139.6917 },
  newyork: { name: 'New York, USA', lat: 40.7128, lon: -74.0060 },
}

// 現在地の選択（初期は日本）
//const currentLocation = ref(locations.japan)

// 現在地の選択（初期はニューヨーク）
const currentLocation = ref(locations.newyork)

// 現在時刻を更新する関数
function updateTime() {
  const now = new Date()
  // 日本時間（UTC+9）に変換
  const japanTime = new Date(now.getTime() + 9 * 60 * 60 * 1000)
  const formatted = japanTime.toISOString().replace('T', ' ').substring(0, 19)
  currentTime.value = formatted
}

//ログ用　初期設定の値の確認用
function init() {



  //取得したURLパラメータの表示
  addLog("URLクエリ" , " mode:" ,urlParamMode, " site:" , urlParamSite, " lang:" , urlParamLang, " date:" , urlParamDate)

  addLog("UTC:",fixedUTCString)
  addLog("観測者の場所:",currentLocation.value.name)

  addLog("高度0判定 観測 skip:",skipFlagAltJudgePictureCanvas.value)
  addLog("高度0判定 星図 skip:",skipFlagAltJudgeStarCanvas.value)


  addLog("初期値の確認完了")
  
}






// マウント時に現在時刻をセット
onMounted(() => {
  updateTime()
  
  // 1秒ごとに更新（任意）
  //setInterval(updateTime, 1000)

  timerId = setInterval(() => {
    updateTime()
    countdown()
  }, 1000)
})

// アンマウント時（画面離脱時） タイマー処理
onUnmounted(() => {
  if (timerId) {
    clearInterval(timerId)
  }
})

// 残り時間を減らす タイマー処理
function countdown() {
  if (logcount.value > 0) {
    logcount.value--
  } else {
    logout()
  }
}




//変換  度 → HH:MM:SS 
function degToHmsString(deg) {
  const absDeg = Math.abs(deg)
  const d = Math.floor(absDeg)
  const m = Math.floor((absDeg - d) * 60)
  const s = Math.floor(((absDeg - d) * 60 - m) * 60)
  const pad = (n) => String(n).padStart(2, '0')
  const sign = deg >= 0 ? '' : '-'
  return `${sign}${pad(d)}:${pad(m)}:${pad(s)}`
}

// 現在の方位角・高度を計算して HH:MM:SS で返す
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

	//方位をトレーニングモードに合わせて180度の加算処理
	azDeg = (azDeg + 180) % 360

  return {
    az: degToHmsString(azDeg),                 // 方位はそのまま
    alt: (altDeg >= 0 ? '+' : '-') + degToHmsString(Math.abs(altDeg)),  // 高度に符号を付与
    altDeg: altDeg   //  判定用の数値
  }
}


</script>
