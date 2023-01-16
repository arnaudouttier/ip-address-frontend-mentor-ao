<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios';
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer } from "@vue-leaflet/vue-leaflet";

const response = ref(null)
let userIp = ref(null)
let isValidIp = true
const zoom = ref(2)

const fetchApiGeo = async () => {

  isValidIp = ipv4Regex.test(userIp.value)

  if (isValidIp) {
    try {
      response.value = await axios.get(`https://geo.ipify.org/api/v2/country,city?apiKey=at_EsRwPew8lc3DElLNSTB2UbMbtP6du&ipAddress=${userIp.value}`)
    }
    catch (err) {
      console.error(err)
    }
  }

  userIp.value = ""
  userIp = ref(null)
}


</script>

<template>


  <div id="map">

    <l-map ref="map" v-model:zoom="zoom" :center="[47.41322, -1.219482]">
      <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" layer-type="base"
        name="OpenStreetMap"></l-tile-layer>
    </l-map>


    <section class="api-input">

      <h1>IP Address Tracker</h1>

      <span class="danger" v-if="!isValidIp">Please add a valid IP address</span>

      <div class="search">

        <input type="text" @keyup.enter="fetchApiGeo" v-model="userIp" :class="{ warning: !isValidIp }"
          placeholder="Search for any IP address or domain" />

        <button class="btn btn-search" :class="{ warning: !isValidIp }" @click="fetchApiGeo">
          <svg xmlns="http://www.w3.org/2000/svg" width="11" height="14">
            <path fill="none" stroke="#FFF" stroke-width="3" d="M2 1l6 6-6 6" />
          </svg>
        </button>

      </div>
      <!-- .search -->

      <ul class="result">
        <li class="list-item">
          <h2 class="item-title">IP ADDRESS</h2>
          <h3 class="item-text">192.212.174.101</h3>
        </li>
        <li class="list-item">
          <h2 class="item-title">LOCATION</h2>
          <h3 class="item-text">Lille, France 59000</h3>
        </li>
        <li class="list-item">
          <h2 class="item-title">TIMEZONE</h2>
          <h3 class="item-text">UTC - 05000</h3>
        </li>
        <li class="list-item">
          <h2 class="item-title">ISP</h2>
          <h3 class="item-text">M2I Le cnam</h3>
        </li>
      </ul>
      <!-- .result -->

    </section>
    <!-- .api-input -->

  </div>
  <!-- #map -->

</template>

<style scoped>
/* class */
.warning {
  outline: 2px solid #CD0404;
}

#map {
  height: 60vh;
  position: relative;
}

.api-input {
  width: min(90vw, 1110px);
  margin-inline: auto;
  display: flex;
  flex-direction: column;
  row-gap: 25px;
  position: absolute;
  top: -64%;
  left: 50%;
  transform: translateX(-50%);
  z-index: 999;
}

h1 {
  font-size: 1.7rem;
  color: #fff;
  margin-bottom: 10px;
}

.api-input .danger {
  color: #CD0404;
}

.search {
  display: flex;
  align-self: center;
  position: relative;
  width: 100%;

}

.search input {
  padding: 20px;
  border-radius: 0.6rem;
  border: 0;
  background-color: #fff;
  flex-grow: 1;
}

.search .btn-search {
  position: absolute;
  top: 0;
  right: -3px;
  bottom: 0;
  border-radius: 0 0.6rem 0.6rem 0;
}

.result {
  background-color: #fff;
  display: grid;
  grid-auto-flow: row;
  gap: 25px;
  border-radius: 1rem;
  padding-block: 20px 30px;
  padding-inline: 0;
}

.result .item-title {
  font-size: .7rem;
  font-weight: 700;
  color: var(--gray_100);
  letter-spacing: 1px;
}

@media(min-width:992px) {
  #map {
    height: 70vh;
  }

  .api-input {
    top: -36%
  }

  .search {
    max-width: 560px;
  }

  .result {
    grid-auto-flow: column;
    grid-auto-columns: 1fr;
    text-align: left;
    padding-block: 40px;
  }

  .result .list-item {
    padding-inline: 30px 0;
  }

  .result .list-item:not(:first-child) {
    border-left: 1px solid rgba(150, 150, 150, 0.31);
  }
}
</style>
