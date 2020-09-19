<template>
  <section class="pomodoro">
    <div class="time-box">
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
    <div class="time-button-wrap">
      <button class="pomodoro-button" @click="openModal">
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
    <transition>
      <div id="overlay" v-show="showModal">
        <div id="modal-wrap">
          <div class="modal-img">
            <img src="@/assets/todo.jpg" class="todo-img" />
          </div>
          <div class="modal-text">
            <table>
              <tr>
                <th class="thr">No.</th>
                <th class="ths">Explain</th>
              </tr>
              <tr>
                <td class="thr">1</td>
                <td class="ths">新規タスクを入力できます</td>
              </tr>
              <tr>
                <td class="thr">2</td>
                <td class="ths">タスクを管理BOXに挿入できます</td>
              </tr>
              <tr>
                <td class="thr">3</td>
                <td class="ths">ダブルクリックで直接編集できます</td>
              </tr>
              <tr>
                <td class="thr">4</td>
                <td class="ths">タスク完了時にチェックできます</td>
              </tr>
              <tr>
                <td class="thr">5</td>
                <td class="ths">タスクを管理BOXから削除できます</td>
              </tr>
              <tr>
                <td class="thr">6</td>
                <td class="ths">ゲージがタイマーと比例進行します</td>
              </tr>
              <tr>
                <td class="thr">7</td>
                <td class="ths">集中モードと休憩モードに分かれます</td>
              </tr>
              <tr>
                <td class="thr">8</td>
                <td class="ths">アプリケーションの説明書を表示します</td>
              </tr>
              <tr>
                <td class="thr">9</td>
                <td class="ths">タイマーを開始、停止できます</td>
              </tr>
              <tr>
                <td class="thr">10</td>
                <td class="ths">タイマーをリセットできます</td>
              </tr>
            </table>
          </div>
          <div class="model-text-responsive">
            <ol class="model-text-ul">
              <li>新規タスクを入力できます</li>
              <li>タスクを管理BOXに挿入できます</li>
              <li>ダブルクリックで直接編集できます</li>
              <li>タスク完了時にチェックできます</li>
              <li>タスクを管理BOXから削除できます</li>
              <li>ゲージがタイマーと比例進行します</li>
              <li>集中モードと休憩モードに分かれます</li>
              <li>アプリケーションの説明書を表示します</li>
              <li>タイマーを開始、停止できます</li>
              <li>タイマーをリセットできます</li>
            </ol>
          </div>
        </div>
        <button @click="closeModal" class="close-button">Close</button>
      </div>
    </transition>
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
      showModal: false,
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
      }, 1000);
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
    openModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
  },
};
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Comfortaa:wght@700&display=swap");

.pomodoro {
  width: 30%;
  margin-right: 4%;
  margin-top: 2%;

  @media screen and(max-width: 480px) {
    width: 100%;
  }
}

.time-box {
  position: relative;
  width: 300px;
  height: 300px;
  margin: 0 auto;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  border-radius: 50%;
  text-align: center;

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

    p {
      margin-top: 94px;
      font-family: "Lora", serif;
      font-family: "Comfortaa", cursive;
      font-weight: 800;
      color: rgb(82, 81, 81);
    }

    span {
      display: block;
      margin-top: 10px;
      font-size: 23px;
      font-weight: 700;
      font-family: "Comfortaa", cursive;
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
  }
}

.time-button-wrap {
  display: flex;
  justify-content: space-around;
  width: 300px;
  height: 100px;
  text-align: center;
  margin-top: 120px;
  margin-left: 10px;
  margin: 0 auto;
  margin-top: 100px;

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

    &:not(:last-child) {
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

      &:hover {
        color: #2928f5;
        opacity: 0.6;
      }
    }
  }
}

#overlay {
  z-index: 999;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(82, 81, 81, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;

  button {
    position: absolute;
    z-index: 99999;
    top: 88%;
    left: 40%;
    padding: 10px 100px;
    border-radius: 20px;
    border: none;
    color: rgb(82, 81, 81);
    font-weight: 600;
    background: #e0e6ec;
    box-sizing: border-box;
    box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
    cursor: pointer;
    transition: all 0.1s;

    @media screen and(max-width: 480px) {
      top: 85%;
      left: 23%;
    }

    &:focus {
      outline: 0;
    }

    &:active,
    &.active {
      outline: 0;
      box-shadow: none;
    }
  }
  #modal-wrap {
    position: relative;
    z-index: 9999;
    width: 90%;
    height: 80%;
    margin: 0 auto;
    padding: 2.5em;
    background: #e0e6ec;
    border-radius: 30px;
    box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
    display: flex;

    @media screen and(max-width: 480px) {
      padding: 1.5rem;
      display: block;
    }

    .modal-img {
      width: 90%;
      margin-right: 20px;

      @media screen and(max-width: 480px) {
        width: 100%;
        margin: 0 auto;
      }
      .todo-img {
        width: 100%;
        height: 95%;
        background: #e0e6ec;
        box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
      }
    }

    .modal-text {
      width: 30%;
      height: 90%;

      @media screen and(max-width: 480px) {
        display: none;
      }

      table {
        width: 100%;
        border-collapse: separate;
        border-spacing: 0;
      }

      table th:first-child {
        border-radius: 5px 0 0 0;
      }
      table th:last-child {
        border-radius: 0 5px 0 0;
      }
      td {
        font-size: 14px;
        color: rgb(82, 81, 81);
      }
      .ths {
        text-align: center;
        border-bottom: 1px solid #a8b7c5;
        box-shadow: 0px -3px 5px 1px #eee inset;
        width: 90%;
        padding: 12px 0;
      }
      .thr {
        text-align-last: center;
        border-bottom: 1px solid #a8b7c5;
        box-shadow: 0px -3px 5px 1px #eee inset;
        width: 12%;
      }
    }
    .model-text-responsive {
      display: none;
      .model-text-ul {
        color: rgba(82, 81, 81, 1);
        width: 90%;
        text-align: left;
        margin: 30px auto;
      }
      li {
        margin-left: 12%;
        margin-bottom: 5px;
      }
      @media screen and(max-width: 480px) {
        display: block;
      }
    }
  }
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s;
}

.v-enter,
.v-leave-to {
  opacity: 0;
}
</style>