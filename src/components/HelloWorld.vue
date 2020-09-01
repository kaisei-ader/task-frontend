<template>
  <div class="hello">
    <div class="textErea">
      <div class="textInput">
        <input type="text" id="input1" v-model="todo" placeholder="Please enter the text !!" />
        <label for="input1">To Do text</label>
      </div>
      <button @click="add" class="addButton">Add</button>
    </div>
    <main>
      <div class="mainBox">
        <div v-for="(todo, index) in todos" :key="index" class="mainWrap">
          <input
            type="checkbox"
            :id="'todo-'+index"
            @change="todo.finish = !todo.finish"
            class="mainCheck"
          />
          <label :class="{'finish':todo.finish}" :for="'todo-'+index" class="mainText">{{todo.name}}</label>
          <button @click="() => remove(index)" class="mainDelate">
            <font-awesome-icon :icon="['far', 'trash-alt']" class="garbege" />
          </button>
        </div>
      </div>
    </main>
    <section class="pomodoro">
      <div class="timebox">
        <div class="inbox">
          <p>
            2 5 : 0 0
            <br />
            <span>session</span>
          </p>
        </div>
      </div>
      <div class="timeButtonWrap">
        <button>?</button>
        <button class="startButton">
          <font-awesome-icon :icon="['fas', 'play-circle']" class="start" />
        </button>
        <button class="resetButton">
          <font-awesome-icon :icon="['fas', 'redo-alt']" class="reset" />
        </button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  methods: {
    add() {
      if (this.todo !== "") {
        this.todos.push({ name: this.todo, finish: false });
        console.log(this.todos);
        this.todo = "";
      }
    },
    remove(index) {
      this.todos = this.todos.filter((v, i) => i !== index);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Sawarabi+Mincho&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Noto+Serif+JP:wght@300;400&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@500;900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Comfortaa:wght@700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Lora:wght@400;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Roboto+Condensed:ital,wght@0,400;1,700&display=swap");
.hello {
  width: 100%;
  height: 100vh;
  background: #e0e6ec;
  padding-top: 100px;
  box-sizing: border-box;
  position: relative;
}
* {
  font-family: "Noto Serif JP", serif;
  font-family: "Noto Sans JP", sans-serif;
}
.finish {
  text-decoration: line-through;
}
.textErea {
  width: 70%;
  height: 150px;
  margin-left: 190px;
  border-radius: 59px;
}
.textInput {
  position: relative;
}
input[type="text"] {
  display: inline-block;
  width: 500px;
  height: 40px;
  margin-top: 30px;
  font-size: 23px;
  text-shadow: 1px 1px 1px #fff;
  box-sizing: border-box;
  outline: none;
  border-radius: 55px;
  border: 1px solid rgba(255, 255, 255, 0.6);
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  padding: 10px 10px 10px 130px;
  transition: all 0.1s ease-out;
}
input[type="text"] + label {
  position: absolute;
  top: 30;
  left: 221px;
  bottom: 0;
  height: 40px;
  line-height: 40px;
  color: rgb(82, 81, 81);
  border-radius: 59px;
  border: 1px double #eee;
  color: #484b57;
  text-shadow: 1px 1px 1px #fff;
  padding: 0 20px;
  font-weight: 600;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  transform: translateZ(0) translateX(0);
  transition: all 0.3s ease-in;
  transition-delay: 0.2s;
}
input[type="text"]:focus + label {
  transform: translateY(-120%) translateX(0%);
  border-radius: 3px;
  transition: all 0.1s ease-out;
}

input[type="text"]:focus {
  padding: 10px;
  transition: all 0.3s ease-out;
  transition-delay: 0.2s;
}
.addButton {
  margin-top: 20px;
  padding: 10px 100px;
  border-radius: 20px;
  border: none;
  color: rgb(82, 81, 81);
  font-weight: 600;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  cursor: pointer;
}
button:focus {
  outline: 0;
}
.addButton:active {
  box-shadow: none;
  position: relative;
  top: 5px;
}
main {
  margin-top: 20px;
  margin-left: 400px;
}
.mainBox {
  width: 690px;
  height: 370px;
  overflow-y: auto;
  overflow-x: hidden;
  border-radius: 30px;
  -ms-overflow-style: none; /* IE, Edge 対応 */
  scrollbar-width: none; /* Firefox 対応 */
}
.mainBox::-webkit-scrollbar {
  /* Chrome, Safari 対応 */
  display: none;
}
.mainWrap {
  position: relative;
  width: 440px;
  margin-top: 20px;
  margin-left: 15px;
  border-radius: 20px;
  padding: 20px 30px;
  display: flex;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
}
.mainText {
  font-size: 16px;
}
.mainDelate {
  position: absolute;
  top: 18px;
  left: 260px;
  background-color: transparent;
  border: none;
  cursor: pointer;
  outline: none;
  padding: 0;
  appearance: none;
  margin-left: 200px;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  border-radius: 50%;
}
.garbege {
  color: #2928f5;
  color: #19189d;
  color: #141471;
  color: #2928f5;
  color: #686b9f;
  color: rgb(82, 81, 81);
  font-size: 20px;
}
input[type="checkbox"] {
  display: none;
}
input[type="checkbox"] + label {
  display: block;
  margin-left: 28px;
  font: 18px/20px "Open Sans", Arial, sans-serif;
  color: rgb(82, 81, 81);
  font-family: "Noto Serif JP", serif;
  font-family: "Noto Sans JP", sans-serif;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}
input[type="checkbox"] + label:before {
  content: "";
  display: block;
  width: 14px;
  height: 14px;
  border: 2px solid #2928f5;
  position: absolute;
  left: 20px;
  top: 21px;
  opacity: 0.6;
  -webkit-transition: all 0.12s, border-color 0.08s;
  transition: all 0.12s, border-color 0.08s;
}
input[type="checkbox"]:checked + label:before {
  width: 8px;
  top: 16px;
  left: 27px;
  border-radius: 0;
  opacity: 1;
  border-top-color: transparent;
  border-left-color: transparent;
  -webkit-transform: rotate(45deg);
  transform: rotate(45deg);
}
label:before {
  position: absolute;
  top: 10px;
}
.pomodoro {
  position: absolute;
  top: 100px;
  left: 990px;
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
  box-shadow: inset 5px 5px 21px #9da1a5, inset -5px -5px 21px #ffffff;
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
.startButton {
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
.start {
  font-size: 50px;
  color: rgb(82, 81, 81);
  color: #686b9f;
  font-weight: 800;
  background: #e0e6ec;
  border-radius: 50%;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
}
.resetButton {
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
.reset {
  font-size: 50px;
  color: rgb(82, 81, 81);
  color: #686b9f;
  font-weight: 800;
  background: #e0e6ec;
  border-radius: 50%;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
}
</style>
