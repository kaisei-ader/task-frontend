<template>
  <div id="app">
    <header>
      <div class="headerWrap">
        <h1>Neumorphism To Do Appcication</h1>
      </div>
    </header>
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Serif+JP:wght@300;400&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@900&display=swap");
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
header {
  width: 100%;
  z-index: 99;
  position: fixed;
}
.headerWrap {
  border-radius: 0 0 35px 35px;
  width: 98%;
  height: 70px;
  margin: 0 auto;
  background: #e0e6ec;
  box-shadow: 7px 7px 14px #bec4c9, -7px -7px 14px #ffffff;
  border-top: none;
  display: flex;
  text-align: center;
}
.headerWrap h1 {
  font-size: 28px;
  font-family: "Noto Serif JP", serif;
  font-family: "Noto Sans JP", sans-serif;
  margin: 0 auto;
  margin-top: 15px;
  color: rgb(82, 81, 81);
}
</style>