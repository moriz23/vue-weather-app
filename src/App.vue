<template>
  <transition name="fade" appear>
    <weather-panel></weather-panel>
  </transition>
</template>

<script>
import WeatherPanel from "./components/layout/WeatherPanel.vue";
export default {
  components: { WeatherPanel },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,400;0,500;0,700;0,900;1,300;1,700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
  background-color: #262c49;
  margin: 30px 30px 25px 30px;
}

#app {
  display: flex;
  justify-content: center;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.fade-enter-active {
  transition: opacity 0.5s ease-out;
}
.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}
.fade-leave-active {
  transition: opacity 0.5s ease-in;
}
</style>
