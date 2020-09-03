<template>
  <div class="hello">
    <div class="todoErea">
      <div class="textErea">
        <div class="textInput">
          <input
            type="text"
            id="input1"
            v-model="todo"
            @keypress.enter="onEnterPress"
            placeholder="Please enter the text !!"
          />
          <label for="input1">To Do text</label>
          <button @click="add" id="addButton">Add</button>
        </div>
      </div>

      <div class="mainBox">
        <div v-for="(todo, index) in todos" :key="index" class="mainWrap">
          <input
            type="checkbox"
            :id="'todo-'+index"
            @change="todo.finish = !todo.finish"
            :class="{'checked':todo.finish}"
          />
          <label :class="{'finish':todo.finish}" :for="'todo-'+index" class="mainText">{{todo.name}}</label>
          <button @click="() => remove(index)" class="mainDelate">
            <font-awesome-icon :icon="['far', 'trash-alt']" class="garbege" />
          </button>
        </div>
      </div>
    </div>
    <Pomodoro />
  </div>
</template>

<script>
import $ from "jquery";
import Pomodoro from "@/components/Pomodoro";
export default {
  name: "HelloWorld",
  components: {
    Pomodoro,
  },
  data() {
    return {
      todo: "",
      todos: [],
      addButton: "addButton",
    };
  },
  methods: {
    add() {
      if (this.todo !== "") {
        this.todos.push({ name: this.todo, finish: false });
        this.todo = "";
      }
    },
    remove(index) {
      this.todos = this.todos.filter((v, i) => i !== index);
    },
    onEnterPress() {
      this.add();
      $("#addButton").addClass("active");
      setTimeout(() => $("#addButton").removeClass("active"), 100);
      $("#input1").blur();
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
  display: flex;
  justify-content: space-between;
}
.todoErea {
  width: 70%;
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
  margin-left: 20%;
  border-radius: 59px;
}
.textInput {
  position: relative;
}
input[type="text"] {
  position: absolute;
  top: 8px;
  left: 3%;
  width: 600px;
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
input[type="text"]:focus {
  color: rgb(82, 81, 81);
}
input[type="text"] + label {
  position: absolute;
  top: 35px;
  left: 3%;
  height: 40px;
  line-height: 40px;
  color: rgb(194, 71, 71);
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
  border-radius: 59px;
  transition: all 0.1s ease-out;
}

input[type="text"]:focus {
  padding: 10px;
  transition: all 0.3s ease-out;
}
#addButton {
  position: absolute;
  top: 102px;
  left: 200px;
  padding: 10px 100px;
  border-radius: 20px;
  border: none;
  color: rgb(82, 81, 81);
  font-weight: 600;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  cursor: pointer;
  transition: all 0.1s;
}
button:focus {
  outline: 0;
}
#addButton.active,
#addButton:active {
  box-shadow: none;
}
main {
  margin-top: 20px;
}
.mainBox {
  width: 750px;
  height: 370px;
  margin-left: 9%;
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
  width: 580px;
  margin-top: 20px;
  margin-left: 13%;
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
  left: 390px;
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
input[type="checkbox"].checked + label:before {
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
</style>
