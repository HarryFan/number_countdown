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
    },
    data() {
      return {
        countdown: this.time, // 倒數的初始時間（秒）
      };
    },
    mounted() {
      this.timer = setInterval(this.tick, 1000); // 每隔1秒鐘執行tick方法
    },
    beforeUnmount() {
      clearInterval(this.timer); // 在元件銷毀前停止計時器
    },
    methods: {
      tick() {
        if (this.countdown > 0) { // 如果倒數時間大於0，則減去1秒
          this.countdown--;
        }
      },
      rest() {
        this.countdown = 60; // 重新設定倒數時間為60秒
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