<script setup>
import IpApiInput from './components/IpApiInput.vue'
</script>

<template>
  <div class="bg-top"></div>
  <IpApiInput />
</template>

<style >
#app {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  text-align: center;
}

.bg-top {
  height: 40vh;
  background-image: url("assets/images/pattern-bg.png");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  position: relative;
  z-index: 99;
}

@media(min-width:992px) {
  .bg-top {
    height: 30vh;
  }
}
</style>
