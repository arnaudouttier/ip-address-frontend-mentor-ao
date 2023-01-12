<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios';

const response = ref(null)

const fetchApiGeo = async () => {
  try {
    response.value = await axios.get("https://geo.ipify.org/api/v2/country,city?apiKey=at_EsRwPew8lc3DElLNSTB2UbMbtP6du&ipAddress=192.169.69.2")
  }
  catch (err) {
    console.error(err)
  }
}

onMounted(() => {
  fetchApiGeo()
})

</script>

<template>

  <div class="map">

    <section class="api-input">

      <h1>IP Address Tracker</h1>

      <div class="search">
        <input type="text" value="IP" placeholder="Search for any IP address or domain">
        <button class="btn btn-search">
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
  <!-- .map -->

</template>

<style scoped>
.map {
  height: 60vh;
  background-image: url("../assets/images/map.png");
  background-repeat: no-repeat;
  background-size: cover;
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
  max-width: 560px;
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
  border-radius: 20px;
  padding-block: 20px 30px;
  padding-inline: 0;
}


.result .item-title {
  font-size: .7rem;
  color: var(--gray_100);
  letter-spacing: 2px;
}

@media(min-width:992px) {
  .map {
    height: 70vh;
  }

  .api-input {
    top: -35%
  }

  .result {
    grid-auto-flow: column;
    text-align: left;
  }


  .result {
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
