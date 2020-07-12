<template>
  <div :class="$style.app">
    <MainContents></MainContents>
  </div>
</template>

<script>
import MainContents from "./components/MainContents";

export default {
  name: "App",
  components: {
    MainContents
  }
};
</script>

<style module>
.app {
  font-family: "游ゴシック体", YuGothic, "游ゴシック Medium", "Yu Gothic Medium",
    "游ゴシック", "Yu Gothic", "メイリオ", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: #e5e5e5;
}
</style>
