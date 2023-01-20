<script setup>
import { ref, onMounted } from "vue";
import leaflet from "leaflet";

let userIp = ref(null);
let isValidIp = ref(true);
let isLoading = ref(false)
let mymap;
let results = ref([]);
let resultsFlag = ref([]);

onMounted(() => {
  mymap = leaflet.map("mymap")
    .setView([48.864716, 2.349014], 13)
    .setZoom(9);
  leaflet
    .tileLayer(
      "https://tile.openstreetmap.org/{z}/{x}/{y}.png",
      {
        attribution:
          '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
      }
    )
    .addTo(mymap);

  mymap.zoomControl.setPosition('bottomright');
});

const validIpAddress = (value) => {
  isValidIp = /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(value);
  return isValidIp
}

const fetchLocation = async () => {
  if (validIpAddress(userIp.value)) {
    try {
      const response = await fetch(`https://ipwho.is/${userIp.value}`);
      const responseData = await response.json();
      results = responseData
      resultsFlag = responseData.flag

      setTimeout(() => {
        leaflet
          .marker([responseData.latitude, responseData.longitude])
          .addTo(mymap);
        mymap.setView(
          [responseData.latitude, responseData.longitude],
          13
        );
      }, 500);
    }
    catch (err) {
      console.error(err);
    }
    isLoading = ref(true)
  }
  else {
    isValidIp = ref(false)
    isLoading = ref(false)
    alert("Please add a valid IP address")
  }
  userIp.value = "";
  userIp = ref(null);
};

</script>

<template>
  <div class="map">

    <div id="mymap"></div>

    <section class="search-box">

      <h1>IP Address Tracker</h1>

      <div class="search">
        <input type="text" @keyup.enter="fetchLocation" v-model="userIp" :class="{ warning: !isValidIp }"
          placeholder="Search for any IP address or domain" />
        <!-- .search -->

        <button class="btn btn-search" :class="{ warning: !isValidIp }" @click="fetchLocation">
          <svg xmlns="http://www.w3.org/2000/svg" width="11" height="14">
            <path fill="none" stroke="#FFF" stroke-width="3" d="M2 1l6 6-6 6" />
          </svg>
        </button>
      </div>
      <!-- .search -->

      <ul id="results">
        <li class="list-item">
          <h2 class="item-title">IP ADDRESS</h2>
          <h3 v-if="isValidIp" class="item-text">{{ results.ip }}</h3>
          <p v-if="!isLoading">...</p>
        </li>
        <li class=" list-item">
          <h2 class="item-title">LOCATION</h2>
          <h3 v-if="isValidIp" class="item-text">
            {{ results.city }} {{ results.city }}
          </h3>
          <p v-if="!isLoading">...</p>
        </li>
        <li class=" list-item">
          <h2 class="item-title">TIMEZONE</h2>
          <h3 v-if="isValidIp" class="item-text">{{
            resultsFlag.emoji_unicode
          }}</h3>
          <p v-if="!isLoading">...</p>
        </li>
        <li class=" list-item">
          <h2 class="item-title">ISP</h2>
          <h3 v-if="isValidIp" class="item-text">{{ results.country_code }}</h3>
          <p v-if="!isLoading">...</p>
        </li>
      </ul>
      <!-- #results -->

    </section>
    <!-- .search-box -->

  </div>
  <!-- .map -->
</template>

<style scoped>
/* class */
.warning {
  outline: 2px solid #cd0404;
}

.map {
  height: 60%;
  position: relative;
}

#mymap {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.search-box {
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
  .map {
    height: 70%;
  }

  .search-box {
    top: -42%;
  }

  .search {
    max-width: 560px;
    margin-bottom: 2rem;
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
