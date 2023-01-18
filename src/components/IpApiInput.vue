<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import leaflet from "leaflet";

let userIp = ref(null);
let isValidIp = true;
let results = ref([]);
let mymap;
let resultsLocation = ref([]);
let latitudeLongitude = ref([51.505, -0.09]);
const zoom = ref(18);

onMounted(() => {
  mymap = leaflet.map("idmap").setView(latitudeLongitude, 13);
  leaflet
    .tileLayer(
      "https://tile.openstreetmap.org/{z}/{x}/{y}.png",
      {
        attribution:
          '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
        maxZoom: 18,
        id: "mapbox/streets-v11",
        tileSize: 512,
        zoomOffset: -1,
      }
    )
    .addTo(mymap);
});

const validIpAddress = (value) => {
  isValidIp = /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(value);
  return isValidIp
}

const fetchLocation = async () => {
  if (validIpAddress(userIp.value)) {
    try {
      let response = await axios
        .get(`http://api.ipapi.com/api/${userIp.value}?access_key=0b10b1155ca8c14e4751b754089c54ce`)
        .then((response) => {
          results = response.data;
          resultsLocation = response.data.location
          console.log(results);
          latitudeLongitude = [results.latitude, results.longitude];
        });
      setTimeout(() => {
        leaflet
          .marker(latitudeLongitude)
          .addTo(mymap);
        mymap.setView(
          latitudeLongitude,
          13
        );
      }, 500);
    }
    catch (err) {
      console.error(err);
    }
  }
  userIp.value = "";
  userIp = ref(null);
};
</script>

<template>
  <div id="idmap">

    <section class="api-input">
      <h1>IP Address Tracker</h1>

      <span class="danger" v-if="!isValidIp">Please add a valid IP address</span>

      <div class="search">
        <input type="text" @keyup.enter="fetchLocation" v-model="userIp" :class="{ warning: !isValidIp }"
          placeholder="Search for any IP address or domain" />

        <button class="btn btn-search" :class="{ warning: !isValidIp }" @click="fetchLocation">
          <svg xmlns="http://www.w3.org/2000/svg" width="11" height="14">
            <path fill="none" stroke="#FFF" stroke-width="3" d="M2 1l6 6-6 6" />
          </svg>
        </button>
      </div>
      <!-- .search -->

      {{ latitudeLongitude }}

      <ul id="results">
        <li class="list-item">
          <h2 class="item-title">IP ADDRESS</h2>
          <h3 class="item-text">{{ results.ip }}</h3>
        </li>
        <li class="list-item">
          <h2 class="item-title">LOCATION</h2>
          <h3 class="item-text">
            {{ results.city }} - {{ results.country_name }}
          </h3>
        </li>
        <li class="list-item">
          <h2 class="item-title">TIMEZONE</h2>
          <h3 class="item-text">{{ resultsLocation.country_flag_emoji_unicode }}</h3>
        </li>
        <li class="list-item">
          <h2 class="item-title">ISP</h2>
          <h3 class="item-text">{{ results.continent_code }}</h3>
        </li>
      </ul>
      <!-- #results -->

    </section>
    <!-- .api-input -->
  </div>
  <!-- #idmap -->
</template>

<style scoped>
/* class */
.warning {
  outline: 2px solid #cd0404;
}

#idmap {
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
  color: #cd0404;
}

.search {
  display: flex;
  align-self: latitudeLongitude;
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

#results {
  background-color: #fff;
  display: grid;
  grid-auto-flow: row;
  gap: 25px;
  border-radius: 1rem;
  padding-block: 20px 30px;
  padding-inline: 0;
}

#results .item-title {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--gray_100);
  letter-spacing: 1px;
}

@media (min-width: 992px) {
  #idmap {
    height: 70vh;
  }

  .api-input {
    top: -36%;
  }

  .search {
    max-width: 560px;
  }

  #results {
    grid-auto-flow: column;
    grid-auto-columns: 1fr;
    text-align: left;
    padding-block: 40px;
  }

  #results .list-item {
    padding-inline: 30px 0;
  }

  #results .list-item:not(:first-child) {
    border-left: 1px solid rgba(150, 150, 150, 0.31);
  }
}
</style>
