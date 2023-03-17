<!-- 
  這個元件是一個倒數計時器，可以在網頁上顯示一個倒數計時器，並且有進度條和音效。
  它有一些屬性，包括倒數的時間（以秒為單位）和是否需要撥放音效。
  當倒數計時器開始運行時，它會在每秒鐘執行一次 "tick" 方法，將倒數計時器的時間減少1秒。
  在倒數時間剩下3秒時，如果需要撥放音效，它會播放音效。當倒數計時器達到0時，它會顯示一個結束提示和一個 "重新開始" 按鈕。
  當 "重新開始" 按鈕被點擊時，它會重新設定倒數時間並重新啟用進度條的過渡效果。
-->
<template>
  <div>
    <div class="countdown-wrapper">
      <div class="countdown">
        <!-- 分鐘數字 -->
        <div class="number-wrapper">
          <div class="number">{{ minutes }}</div>
        </div>
        <!-- 冒號分隔符號 -->
        <div class="separator">:</div>
        <!-- 秒數十位數字 -->
        <div class="number-wrapper">
          <div class="number">{{ tenSeconds }}</div>
        </div>
        <!-- 秒數個位數字 -->
        <div class="number-wrapper">
          <div class="number">{{ seconds }}</div>
        </div>

        <!-- 倒數計時結束提示 -->
        <div v-if="countdown <= 0" class="countdown-finished">
          時間到了！
          <button @click="reset" class="rest-button">重新開始</button>
        </div>
      </div>
    </div>

    <div class="countdown-wrapper-2">
      <div class="countdown">
        <!-- 進度條 -->
        <div class="progress-bar-wrap">
          <div class="progress-bar" :style="{ width: progressBarWidth + '%' }"></div>
        </div>
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
        timer: null, // 計時器物件
      };
    },
    computed: {
      // 分鐘數
      minutes() {
        return Math.floor(this.countdown / 60);
      },
      // 秒數（十位數）
      tenSeconds() {
        return Math.floor(this.countdown % 60 / 10);
      },
      // 秒數（個位數）
      seconds() {
        return Math.floor(this.countdown % 10);
      },
      // 進度條寬度
      progressBarWidth() {
        return ((this.countdown / this.time) * 100);
      }
    },
    mounted() {
      // 每秒呼叫tick方法
      this.timer = setInterval(this.tick, 1000);
      // 載入音效
      this.audio = new Audio(require('@/assets/sounds/countdown.mp3'));
      this.audio.load();
    },
    beforeUnmount() {
      // 在元件銷毀前停止計時器
      clearInterval(this.timer);
    },
    methods: {
      // 撥放音效
      playAudio() {
        const playPromise = this.audio.play();
        if (playPromise !== undefined) {
          playPromise.catch(error => {
            console.error('Failed to play audio:', error);
          });
        }
      },
      // tick方法
      tick() {
        // 如果倒數時間大於0，則減去1秒
        if (this.countdown > 0) {
          this.countdown--;
          // 如果需要撥放音效，且倒數時間剩下3秒時，撥放音效
          if (this.sound && this.countdown === 3) {
            this.playAudio();
          }
        }
      },
      // 重新開始
      reset() {
        // 重新設定倒數時間為60秒
        this.countdown = this.time;
        // 暫時取消進度條的過渡效果
        document.querySelector(".progress-bar").style.transition = "none";
        // 在下一個畫面更新周期重新啟用過渡效果
        this.$nextTick(() => {
          document.querySelector(".progress-bar").style.transition = "width 0.5s";
        });
      }
    }
  };
</script>

<style scoped>
  /* 將元素水平置中 */
  .countdown-wrapper {
    display: flex;
    justify-content: center;
    margin: 1em 0;
  }

  .countdown-wrapper-2 {
    display: flex;
    justify-content: center;
    margin: 1em 0;
  }

  /* 將元素垂直置中 */
  .countdown {
    display: flex;
    align-items: center;
    width: 500px;
    justify-content: center;
  }

  /* 數字外框樣式 */
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

  /* 冒號分隔符號樣式 */
  .separator {
    font-size: 80px;
    font-weight: bold;
    color: #000;
  }

  /* 倒數計時結束提示樣式 */
  .countdown-finished {
    font-size: 24px;
    font-weight: bold;
    color: #ff0000;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* 重新開始按鈕樣式 */
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

  /* 進度條外框樣式 */
  .progress-bar-wrap {
    width: 100%;
    height: 20px;
    background-color: #e0e0e0;
    border-radius: 10px;
    margin: 1em 0;
  }

  /* 進度條樣式 */
  .progress-bar {
    height: 100%;
    background-image: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet);
    border-radius: 10px;
    transition: width 0.5s;
  }
</style>