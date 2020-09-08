<template>
  <div class="hello">
    <div class="todoArea">
      <div class="textArea">
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
        <div v-for="todo in todos" :key="todo.id" class="mainWrap">
          <input
            type="checkbox"
            :id="'todo-'+todo.id"
            @change="() =>updateStatus(todo.id,todo.is_finished)"
            v-model="todo.is_finished"
            :class="{'checked':todo.is_finished}"
          />
          <label
            :class="{'finish':todo.is_finished}"
            :for="'todo-'+todo.id"
            class="mainText"
          >{{todo.name}}</label>
          <button @click="() => remove(todo.id)" class="mainDelete">
            <font-awesome-icon :icon="['far', 'trash-alt']" class="garbage" />
          </button>
        </div>
      </div>
    </div>
    <Pomodoro />
  </div>
</template>

<script>
import $ from "jquery";
import axios from "axios";
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
  created() {
    this.get();
  },
  methods: {
    get() {
      console.log(process.env);
      axios.get(process.env.VUE_APP_API_URL + "/api/task").then((res) => {
        this.todos = res.data;
      });
    },
    add() {
      if (this.todo !== "") {
        axios
          .post(process.env.VUE_APP_API_URL + "/api/task", { name: this.todo })
          .then(() => {
            this.get();
          });
        this.todo = "";
      }
    },
    remove(id) {
      axios.delete(process.env.VUE_APP_API_URL + "/api/task/" + id).then(() => {
        this.get();
      });
    },

    updateStatus(id, isFinished) {
      axios.patch(process.env.VUE_APP_API_URL + "/api/task/" + id, {
        key: "is_finished",
        value: isFinished,
      });
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
<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@500;900&display=swap");

* {
  font-family: "Noto Sans JP", sans-serif;
}

.hello {
  width: 100%;
  height: 100vh;
  background: #e0e6ec;
  padding-top: 100px;
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;

  @media screen and(max-width: 480px) {
    display: block;
    height: 100%;
  }
}

.todoArea {
  width: 70%;

  @media screen and(max-width: 480px) {
    width: 100%;
  }
}

.finish {
  text-decoration: line-through;
}

.textArea {
  width: 70%;
  height: 150px;
  margin-left: 20%;
  border-radius: 59px;
  @media screen and(max-width: 480px) {
    width: 100%;
    margin-left: 0;
  }
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
  &:focus {
    color: rgb(82, 81, 81);
    padding: 10px;
    transition: all 0.3s ease-out;
  }
  & + label {
    position: absolute;
    top: 35px;
    left: 3%;
    height: 40px;
    line-height: 40px;
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
    @media screen and(max-width:480px) {
      position: absolute;
      top: 15px;
      left: 0;
    }
  }
  &:focus + label {
    transform: translateY(-120%) translateX(0%);
    border-radius: 59px;
    transition: all 0.1s ease-out;
  }
  @media screen and(max-width: 480px) {
    position: absolute;
    top: 8px;
    left: 0;
    width: 100%;
    margin-top: 10px;
    box-sizing: border-box;
  }
}
#addButton {
  position: absolute;
  top: 101px;
  left: 30%;
  padding: 10px 100px;
  border-radius: 20px;
  border: none;
  color: rgb(82, 81, 81);
  font-weight: 600;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  cursor: pointer;
  transition: all 0.1s;
  &:focus {
    outline: 0;
  }
  &:active,
  &.active {
    outline: 0;
    box-shadow: none;
  }
  @media screen and(max-width: 480px) {
    position: absolute;
    top: 88px;
    left: 100px;
    padding: 10px 100px;
  }
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
  &::-webkit-scrollbar {
    /* Chrome, Safari 対応 */
    display: none;
  }
  @media screen and(max-width: 480px) {
    width: 80%;
    height: 400px;
    margin: 0 auto;
    margin-bottom: 100px;
  }
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
  @media screen and (max-width: 480px) {
    position: relative;
    width: 80%;
    margin-top: 27px;
    margin-left: 5px;
  }
}
.mainDelete {
  position: absolute;
  top: 19px;
  left: 390px;
  border: none;
  cursor: pointer;
  outline: none;
  padding: 0;
  appearance: none;
  margin-left: 200px;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  border-radius: 50%;
  .garbage {
    color: rgb(82, 81, 81);
    font-size: 20px;
  }
  @media screen and(max-width:480px) {
    left: 0px;
    margin-left: 80%;
  }
}
input[type="checkbox"] {
  display: none;
  & + label {
    display: block;
    margin-left: 28px;
    font: 18px/20px "Open Sans", Arial, sans-serif;
    color: rgb(82, 81, 81);
    font-family: "Noto Sans JP", sans-serif;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
  }
  & + label:before {
    content: "";
    display: block;
    width: 14px;
    height: 14px;
    border: 2px solid #2928f5;
    position: absolute;
    left: 20px;
    top: 22px;
    opacity: 0.6;
    -webkit-transition: all 0.12s, border-color 0.08s;
    transition: all 0.12s, border-color 0.08s;
  }
  @media screen and(max-width: 480px) {
    display: none;
    margin-left: 28px;
    font: 15px/20px "Open Sans", Arial, sans-serif;
  }
  &.checked + label:before {
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
}
</style>
