<template>
  <section class="pomodoro">
    <div class="timebox">
      <div class="inbox">
        <p>
          {{ formatTime }}
          <span>session</span>
        </p>
        <p v-if="work" class="working-text">Working Time</p>
        <p v-if="!work" class="breaking-text">Break Time</p>
      </div>
      <svg
        width="307"
        height="307"
        viewBox="0, 0, 200, 200"
        xmlns="http://www.w3.org/2000/svg"
        class="svganime"
      >
        <circle cx="100" cy="100" r="90" />
      </svg>
    </div>
    <div class="timeButtonWrap">
      <button class="pomodoro-button">
        <font-awesome-icon :icon="['fas', 'question-circle']" class="pomodoro-button-icon" />
      </button>
      <button class="pomodoro-button" v-if="!timerOn" @click="start" data-b="start">
        <font-awesome-icon :icon="['fas', 'play-circle']" class="pomodoro-button-icon" />
      </button>
      <button class="pomodoro-button" v-if="timerOn" @click="stop" data-b="stop">
        <font-awesome-icon :icon="['fas', 'pause-circle']" class="pomodoro-button-icon" />
      </button>
      <button class="pomodoro-button" @click="reset">
        <font-awesome-icon :icon="['fas', 'redo-alt']" class="pomodoro-button-icon" />
      </button>
    </div>
  </section>
</template>

<script>
import $ from "jquery";
export default {
  name: "Pomodoro",
  data() {
    return {
      timerObj: null,
      timerOn: false,
      work: true,
      min: 25,
      sec: 0,
    };
  },
  computed: {
    formatTime() {
      let zeroPadding = (v) => ("0" + v.toString()).slice(-2);
      return zeroPadding(this.min) + ":" + zeroPadding(this.sec);
    },
  },
  methods: {
    count() {
      if (this.min === 0) {
        if (this.sec === 0) {
          if (this.work) {
            this.min = 5;
          } else {
            this.min = 25;
          }
          this.sec = 0;
          this.work = !this.work;
        }
      }
      if (this.sec === 0) {
        this.min--;
        this.sec = 59;
      } else {
        this.sec--;
      }
      $(".svganime").css(
        "stroke-dasharray",
        565 * (1 - (this.min * 60 + this.sec) / (this.work ? 1500 : 300)) +
          " 565"
      );
    },

    start() {
      this.timerObj = setInterval(() => {
        this.count();
      }, 10);
      this.timerOn = true;
    },
    stop() {
      clearInterval(this.timerObj);
      this.timerOn = false;
    },
    reset() {
      this.work = true;
      this.min = 25;
      this.sec = 0;
      this.stop();
      $(".svganime").css(
        "stroke-dasharray",
        565 * (1 - (this.min * 60 + this.sec) / (this.work ? 1500 : 300)) +
          " 565"
      );
    },
  },
};
</script>

<style scoped>
.pomodoro {
  width: 30%;
  margin-right: 4%;
  margin-top: 2%;
}
.timebox {
  position: relative;
  width: 300px;
  height: 300px;
  margin: 0 auto;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  border-radius: 50%;
  text-align: center;
}
.inbox {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  -webkit-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  width: 250px;
  height: 250px;
  border-radius: 50%;
  background: #e0e6ec;
  box-shadow: inset 7px 7px 14px #bec4c9, inset -7px -7px 14px #ffffff;
  font-size: 55px;
  text-align: center;
}
.inbox p {
  margin-top: 94px;
  font-family: "Lora", serif;
  font-family: "Comfortaa", cursive;
  font-weight: 800;
  color: rgb(82, 81, 81);
}
.inbox span {
  display: block;
  margin-top: 10px;
  font-size: 23px;
  font-weight: 700;
  font-family: "Comfortaa", cursive;
}
.timeButtonWrap {
  display: flex;
  justify-content: space-around;
  width: 300px;
  height: 100px;
  text-align: center;
  margin-top: 120px;
  margin-left: 10px;
  margin: 0 auto;
  margin-top: 100px;
}
.pomodoro-button {
  width: 70px;
  height: 70px;
  border: none;
  cursor: pointer;
  outline: none;
  padding: 0;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  border-radius: 50%;
}
.pomodoro-button:not(:last-child) {
  margin-right: 30px;
}
.pomodoro-button-icon {
  font-size: 50px;
  color: rgb(82, 81, 81);
  color: #686b9f;
  color: #2928f5;
  color: rgb(82, 81, 81);
  font-weight: 800;
  background: #e0e6ec;
  border-radius: 50%;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  transition: all 0.6s;
}
.pomodoro-button-icon:hover {
  color: #2928f5;
  opacity: 0.6;
}
.working-text {
  position: absolute;
  top: 220px;
  left: 25px;
  font-size: 28px;
  font-weight: 800;
}
.breaking-text {
  position: absolute;
  top: 220px;
  left: 43px;
  font-size: 28px;
}
.svganime {
  position: absolute;
  left: -3px;
  top: -2px;
  stroke: #2928f5;
  fill: none;
  stroke-width: 7;
  stroke-linecap: round;
  stroke-opacity: 0.6;
  transform: rotate(-90deg);
  stroke-dasharray: 0 723;
  /* animation: line 2500s linear; */
}
/* @keyframes line {
  0% {
    stroke-dasharray: 0 723;
  }
  100% {
    stroke-dasharray: 723 723;
  }
} */
@media screen and (max-width: 480px) {
  .pomodoro {
    width: 100%;
  }
}
</style>
