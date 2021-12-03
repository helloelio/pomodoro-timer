<template>
  <input
    max="59"
    type="number"
    placeholder="Type time in minutes"
    @keypress.enter="setTime"
  />
  <div class="circle">
    <div v-if="this.timer === 0" class="time">
      <img src="./assets/images/check.svg" />
    </div>
    <div v-else class="time">00:{{ formatedSeconds }}</div>
    <div class="buttons">
      <button v-if="this.timer !== 0" @click="turnTimer">
        {{ this.timerStatus ? 'STOP' : 'START' }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      timerStatus: false,
      timer: 0,
    };
  },
  computed: {
    timerCount() {
      return this.timer;
    },
    formatedSeconds() {
      return this.timer < 10
        ? `0${((parseInt(this.timer) * 60) / 60) % 60}`
        : ((parseInt(this.timer) * 60) / 60) % 60;
    },
  },
  /* eslint-disable */
  methods: {
    setTime(event) {
      this.timer = Number(event.target.value);
    },
    turnTimer() {
      this.timerStatus = !this.timerStatus;
      if (!this.timerStatus) {
        return;
      }
      let aboba = setInterval(() => {
        if (!this.timerStatus || this.timer <= 0) {
          clearInterval(aboba);
        }
        this.timer = this.timer - 1;

        if (this.timer === 0) {
          clearInterval(aboba);
          setTimeout(() => {
            this.timer = this.timerCount;
            this.timerStatus = false;
          }, 3000);
        }
      }, 1000);
    },
  },
};
</script>

<style lang="scss">
@font-face {
  font-family: Bebas Neue;
  src: url('./assets/fonts/bebasneue-book-webfont.woff2');
}
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap');
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
button {
  font-family: 'Montserrat', sans-serif;
  font-weight: bold;
  font-size: 16px;
  line-height: 20px;
  letter-spacing: 0.6em;
  text-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  background: none;
  border: none;
  color: white;
  cursor: pointer;
}
#app {
  font-family: Bebas Neue;
  background-color: #2b2a30;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  .circle {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    color: white;
    width: 500px;
    height: 500px;
    border-radius: 50%;
    background: radial-gradient(
      71.4% 71.4% at 51.7% 28.6%,
      #3a393f 0%,
      #17171a 100%
    );
    box-shadow: inset 0px 0px 114px rgba(0, 0, 0, 0.45);
    .time {
      font-size: 196px;
      line-height: 196px;
      text-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    }
  }
}
</style>
