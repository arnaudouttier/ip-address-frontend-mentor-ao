<script setup>
import IpApiInput from './components/IpApiInput.vue'

defineProps({
  message: String,
})
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
}

@media(min-width:992px) {
  .bg-top {
    height: 30vh;
  }
}
</style>
