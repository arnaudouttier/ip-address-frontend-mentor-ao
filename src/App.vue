<script setup>
import SearchMap from './components/SearchMap.vue'
</script>

<template>
  <div class="bg-top"></div>
  <SearchMap />
</template>

<style >
#app {
  width: 100vw;
  height: inherit;
  margin: 0;
  padding: 0;
  text-align: center;
}

.bg-top {
  height: 40%;
  background-image: url("assets/images/pattern-bg.png");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: right;
  position: relative;
  z-index: 99;
}

@media(min-width:992px) {
  .bg-top {
    height: 30%;
  }
}
</style>
