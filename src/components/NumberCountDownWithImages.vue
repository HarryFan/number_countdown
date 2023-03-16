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
        時間到了！
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        countdown: 60, // 倒數的初始時間（秒）
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
      getImagePath(number) {
        return require(`@/assets/numbers/${number}.png`); // 動態生成數字圖片路徑
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
</style>