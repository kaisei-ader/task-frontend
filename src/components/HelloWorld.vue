<template>
  <div class="hello">
    <div class="todo-area">
      <div class="text-area">
        <div class="text-new-input">
          <input
            type="text"
            id="new-input"
            v-model="newTodo"
            @keypress.enter="onEnterPress"
            placeholder="Please enter the text!!"
          />
          <label for="new-input">To Do text</label>
          <button @click="add" id="add-button">Add</button>
        </div>
      </div>
      <div class="main-box-wrap">
        <div v-for="todo in todos" :key="todo.id" class="main-box-content">
          <input
            type="checkbox"
            :id="'todo-'+todo.id"
            @change="() =>updateStatus(todo.id,todo.is_finished)"
            v-model="todo.is_finished"
            :class="{'checked':todo.is_finished}"
          />
          <label :for="'todo-'+todo.id"></label>
          <p
            @dblclick="refbank(todo)"
            v-show="!todo.edit"
            :class="{'finish':todo.is_finished}"
            :for="'todo-'+todo.id"
          >{{todo.name}}</p>
          <input
            type="text"
            :ref="'focus-' + todo.id"
            v-show="todo.edit"
            v-model="todo.name"
            @blur="updateName(todo)"
            class="update-name-text"
          />
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
      newTodo: "",
      todos: [],
    };
  },
  created() {
    this.get();
  },
  methods: {
    get() {
      axios.get(process.env.VUE_APP_API_URL + "/api/task").then((res) => {
        res.data.map((value) => {
          value.edit = false;
        });
        this.todos = res.data;
      });
    },
    add() {
      if (this.newTodo !== "") {
        axios
          .post(process.env.VUE_APP_API_URL + "/api/task", {
            name: this.newTodo,
          })
          .then(() => {
            this.get();
          });
        this.newTodo = "";
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
      $("#add-button").addClass("active");
      setTimeout(() => $("#add-button").removeClass("active"), 100);
      $("#new-input").blur();
    },
    refbank(todo) {
      todo.edit = !todo.edit;
      this.$nextTick(() => this.$refs["focus-" + todo.id][0].focus());
    },
    updateName(todo) {
      if (todo.name !== "") {
        axios.patch(process.env.VUE_APP_API_URL + "/api/task/" + todo.id, {
          key: "name",
          value: todo.name,
        });
      }
      if (todo.name !== "") {
        todo.edit = !todo.edit;
      }
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

.todo-area {
  width: 70%;

  @media screen and(max-width: 480px) {
    width: 100%;
  }
}

.finish {
  text-decoration: line-through;
}

.text-area {
  width: 70%;
  height: 150px;
  margin-left: 20%;
  border-radius: 59px;

  @media screen and(max-width: 480px) {
    width: 100%;
    margin-left: 0;
  }
}
.text-new-input {
  position: relative;
}
.text-new-input [type="text"] {
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
    top: 37px;
    left: 3%;
    box-sizing: border-box;
    height: 41px;
    display: flex;
    align-items: center;
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
      top: 17px;
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

.update-name-text {
  width: 100%;
  text-shadow: 1px 1px 1px #fff;
  box-sizing: border-box;
  outline: none;
  border: none;
  color: rgb(82, 81, 81);
  font: 18px/20px "Noto Sans JP", sans-serif;
  background: #e0e6ec;
}

#add-button {
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

.main-box-wrap {
  width: 750px;
  height: 370px;
  margin-left: 9%;
  overflow-y: auto;
  overflow-x: hidden;
  border-radius: 30px;
  -ms-overflow-style: none; /* IE, Edge 対応 */
  scrollbar-width: none; /* Firefox 対応 */

  .main-box-content {
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
      width: 90%;
      box-sizing: border-box;
      margin-top: 27px;
      margin-left: auto;
      margin-right: auto;
      margin-bottom: 10px;
    }
    p {
      font: 18px/20px "Open Sans", Arial, sans-serif;
      color: rgb(82, 81, 81);
      font-family: "Noto Sans JP", sans-serif;
      cursor: pointer;
    }
  }

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
      margin-left: 85%;
    }
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
    top: 17px;
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
