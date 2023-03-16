<!-- 
這個程式是一個倒數計時器，使用者可以設定倒數時間和是否需要播放音效。畫面上會顯示分、秒數字，倒數結束後會提示使用者時間到了，並且提供重新開始的按鈕。使用者可以透過這個程式方便地進行倒數計時。

在程式中，我們使用 props 來傳入倒數時間和是否需要播放音效。使用 data 來存儲倒數計時的時間和音效檔案物件。在 mounted 週期中，我們會設置一個計時器，每隔1秒呼叫 tick 方法進行倒數計時。同時，我們會載入音效檔案並且進行加載。

在程式中，我們使用了一個 tick 方法來進行倒數計時，同時如果需要播放音效，還會在剩下3秒時撥放。我們還提供了一個 rest 方法，當使用者點擊重新開始按鈕時，會重置倒數計時的時間。最後，我們還提供了樣式設置，使程式的畫面更加美觀。 
-->
<template>
  <div class="countdown-wrapper">
    <div class="countdown">
      <!-- 分鐘數字 -->
      <div class="number-wrapper">
        <div class="number">{{ Math.floor(countdown / 60) }}</div>
      </div>
      <!-- 冒號分隔符號 -->
      <div class="separator">:</div>
      <!-- 秒數十位數字 -->
      <div class="number-wrapper">
        <div class="number">{{ Math.floor(countdown % 60 / 10) }}</div>
      </div>
      <!-- 秒數個位數字 -->
      <div class="number-wrapper">
        <div class="number">{{ Math.floor(countdown % 10) }}</div>
      </div>
      <!-- 倒數計時結束提示 -->
      <div v-if="countdown <= 0" class="countdown-finished">
        時間到了！
        <button @click="rest" class="rest-button">重新開始</button> <!-- rest按鈕 -->
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
      this.timer = setInterval(this.tick, 1000); // 每秒呼叫tick方法
      this.loadAudio(); // 載入音效
    },
    beforeUnmount() {
      clearInterval(this.timer); // 在元件銷毀前停止計時器
    },
    methods: {
      loadAudio() {
        this.audio = new Audio(require('@/assets/sounds/countdown.mp3')); // 載入音效檔案
        this.audio.load(); // 加載音效

      },
      playAudio() {
        const playPromise = this.audio.play(); // 播放音效
        if (playPromise !== undefined) {
          playPromise.catch(error => {
            console.error('Failed to play audio:', error);
          });
        }
      },
      tick() {
        if (this.countdown > 0) { // 如果倒數時間大於0，則減去1秒
          this.countdown--;
          if (this.sound && this.countdown === 3) { // 如果需要撥放音效，且倒數時間剩下3秒時，撥放音效
            this.playAudio();
          }
        }
      },
      rest() {
        this.countdown = this.time; // 重新設定倒數時間為60秒
      }
    },
  };
</script>

<style scoped>
  /* 將元素水平置中 */
  .countdown-wrapper {
    display: flex;
    justify-content: center;
    margin: 1em 0;
  }

  /* 將元素垂直置中 */
  .countdown {
    display: flex;
    align-items: center;
  }

  .number-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
    height: 100px;
    background-color: #fff;
    border-radius: 10px;
    font-size: 80px;
    font-weight: bold;
    color: #000;
    margin: 0 0.3rem;
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
  }

  .separator {
    font-size: 80px;
    font-weight: bold;
    color: #000;
  }

  .countdown-finished {
    font-size: 24px;
    font-weight: bold;
    color: #ff0000;
    display: flex;
    flex-direction: column;
    align-items: center;
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

