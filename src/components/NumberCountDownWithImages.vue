
<!-- 
這個程式是一個倒數計時器，會顯示剩餘時間的分、秒數字圖片，並且會根據剩餘時間撥放音效。

程式會根據傳入的 props 設定倒數時間和是否要撥放音效，若需要撥放音效則會在剩下 3 秒時播放，並且可以點擊按鈕重新開始計時。

透過這個程式，使用者可以方便地設定倒數計時的時間和音效，並且可以在倒數計時過程中清晰地看到剩餘時間。程式還使用了動態生成數字圖片路徑的方式顯示數字圖片，提高了可重用性和可讀性。 
-->

<template>
  <div class="countdown-wrapper">
    <div class="countdown">
      <!-- 分鐘數字圖片 -->
      <div class="img-wrapper">
        <img :src="getImagePath(Math.floor(countdown / 60))" alt="minutes" />
      </div>
      <!-- 冒號分隔符號 -->
      <div class="separator-wrapper">
        <span class="separator">:</span>
      </div>
      <!-- 秒數十位數字圖片 -->
      <div class="img-wrapper">
        <img :src="getImagePath(Math.floor(countdown % 60 / 10))" alt="tens" />
      </div>
      <!-- 秒數個位數字圖片 -->
      <div class="img-wrapper">
        <img :src="getImagePath(Math.floor(countdown % 10))" alt="ones" />
      </div>
      <!-- 倒數計時結束提示 -->
      <div v-if="countdown <= 0" class="countdown-finished">
        <div>時間到了！</div>
        <button class="rest-button" @click="reset">重新開始</button>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    props: {
      // 倒數時間（秒）
      time: {
        type: Number,
        required: true,
        default: 60,
      },
      // 是否要撥放音效
      sound: {
        type: Boolean,
        required: false,
        default: false,
      },
    },
    data() {
      return {
        countdown: this.time, // 倒數的初始時間（秒）
        audio: null, // 音效檔案物件
      };
    },
    mounted() {
      this.loadAudio(); // 在元件加載時加載音效檔案
      this.timer = setInterval(this.tick, 1000); // 每隔1秒鐘執行tick方法
    },
    beforeUnmount() {
      clearInterval(this.timer); // 在元件銷毀前停止計時器
    },
    methods: {
      tick() {
        if (this.countdown > 0) { // 如果倒數時間大於0，則減去1秒
          this.countdown--;
          if (this.sound && this.countdown === 3) { // 如果要撥放音效，且剩下3秒時
            this.playAudio(); // 播放音效
          }
        }
      },
      getImagePath(number) {
        return require(`@/assets/numbers/${number}.png`); // 動態生成數字圖片路徑
      },
      reset() {
        this.countdown = this.time; // 重新設定倒數時間為初始值
      },
      loadAudio() {
        this.audio = new Audio(require('@/assets/sounds/countdown.mp3')); // 加載音效檔案
        this.audio.load(); // 載入音效檔案
      },
      async playAudio() {
        try {
          await this.audio.play(); // 播放音效
        } catch (error) {
          console.error('Failed to play audio:', error); // 處理音效檔案播放的異常情況
        }
      },
    },
  };
</script>

<style scoped>
  /* 將元素水平置中 */
  .countdown-wrapper {
    display: flex;
    justify-content: center;
  }

  /* 將元素水平置中 */
  .countdown {
    display: flex;
    align-items: center;
  }

  .countdown .img-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .countdown img {
    width: 100px;
    height: 100px;
  }

  .separator-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 0.3rem;
    font-size: 80px;
    font-weight: bold;
    color: #000;
  }

  .countdown-finished {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: bold;
    color: #ff0000;
  }

  .rest-button {
    background-color: #4caf50;
    border: none;
    color: #fff;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    font-size: 16px;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 10px;
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
    transition: background-color 0.3s;
  }

  .rest-button:hover {
    background-color: #3e8e41;
  }
</style>
