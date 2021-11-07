<template>
  <div id="main">
    <div class="container">
      <div class="row gx-5">
        <div class="col-4">
          <div class="search">
            <form class="search__location">
              <input
                type="text"
                class="
                  form-control
                  text-muted
                  form-rounded
                  p-2
                  text-center
                  shadow-sm
                "
                v-model="placeSearch"
                placeholder="What city/country?"
                autocomplete="off"
              />
              <p class="place-not-found" v-show="placeNotFound">
                Place not found!
              </p>
              <button
                type="button"
                class="btn btn-outline-success"
                @click.prevent="getCoords"
              >
                Find
              </button>
            </form>
          </div>
        </div>

        <div class="col" v-show="placesShow">
          <div class="variants">
            <div class="variants__title">
              Варианты мест для {{ this.placeSearch }}
            </div>
            <ul class="variants__buttons">
              <button
                type="button"
                class="btn btn-outline-primary p-1"
                v-for="(place, id) in cityPlacesInfo"
                :key="id"
                @click.prevent="
                  getWeather(place.lat, place.lng);
                  getLandmarks(place.lat, place.lng);
                "
              >
                {{ place.name }}
              </button>
            </ul>
            <ul class="variants__coords">
              <li class="coord" v-for="(coor, id) in cityPlacesInfo" :key="id">
                lat: {{ coor.lat }}<br />lng: {{ coor.lng }}
              </li>
            </ul>
          </div>
        </div>

        <div class="col-12">
          <div class="weather" v-show="weatherShow">
            <div class="weather__descr">{{ weatherInfo.descr }}</div>
            <div class="weather__feels">
              feels: {{ weatherInfo.feels }} &#8451;
            </div>
            <div class="weather__temp">{{ weatherInfo.temp }} &#8451;</div>
            <div class="weather__humid">humid: {{ weatherInfo.humid }}</div>
            <div class="weather__wind">wind: {{ weatherInfo.wind }}</div>
          </div>
          <ul class="places" v-show="landmarksShow">
            <div
              class="places__info"
              v-for="(info, id) in landmarksInfo"
              :key="id"
            >
              <div class="name">{{ info.name }}</div>
              <div class="descr">{{ info.descr }}</div>
              <div class="rate">Оценка: {{ info.rate }}</div>
            </div>
          </ul>
          <div class="no-landmarks" v-show="landmarksNotFound">
            <p>Увы, но поблизости нет ничего интересного &#128532;</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      placeSearch: "",
      placesShow: false,
      placeNotFound: false,
      weatherShow: false,
      landmarksShow: false,
      landmarksNotFound: false,

      cityPlacesInfo: [
        { id: 0, name: "???", lat: "???", lng: "???" },
        { id: 1, name: "???", lat: "???", lng: "???" },
        { id: 2, name: "???", lat: "???", lng: "???" },
        { id: 3, name: "???", lat: "???", lng: "???" },
        { id: 4, name: "???", lat: "???", lng: "???" },
      ],

      weatherInfo: {
        descr: "???",
        feels: "?",
        temp: "?",
        humid: "?",
        wind: "?",
      },

      landmarksInfo: [
        {
          id: 0,
          name: "???",
          descr: "Здесь могла быть ваша реклама",
          rate: "?",
        },
        {
          id: 1,
          name: "???",
          descr: "Здесь могла быть ваша реклама",
          rate: "?",
        },
        {
          id: 2,
          name: "???",
          descr: "Здесь могла быть ваша реклама",
          rate: "?",
        },
        {
          id: 3,
          name: "???",
          descr: "Здесь могла быть ваша реклама",
          rate: "?",
        },
        {
          id: 4,
          name: "???",
          descr: "Здесь могла быть ваша реклама",
          rate: "?",
        },
      ],
    };
  },
  methods: {
    getCoords: async function () {
      this.placeNotFound = false;
      const keyAPI = "106696ea-af04-4082-b263-4e4fff5b5d0c";
      const coordsURL = `https://graphhopper.com/api/1/geocode?q=${this.placeSearch}&locale=de&debug=true&limit=20&key=${keyAPI}`;
      const response = await fetch(coordsURL);
      const data = await response.json();
      if (data.hits.length <= 0) {
        this.placeNotFound = true;
        return;
      }
      let j = 0;
      for (let i = 0; i < 5; ++i) {
        this.cityPlacesInfo[i].name = data.hits[j].name;
        this.cityPlacesInfo[i].lat = data.hits[j].point.lat.toFixed(7);
        this.cityPlacesInfo[i].lng = data.hits[j].point.lng.toFixed(7);
        if (data.hits.length >= 9) {
          j += 2;
        } else {
          j++;
        }
      }
      this.placesShow = true;
    },

    getWeather: async function (lat, lng) {
      const keyAPI = "fc66e5ec568a2bd3c45c86a9b418c2c2";
      const weatherURL = `http://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lng}&units=metric&appid=${keyAPI}`;
      const response = await fetch(weatherURL);
      const data = await response.json();
      this.weatherInfo.descr = data.weather[0].description;
      this.weatherInfo.feels = Math.round(data.main.feels_like);
      this.weatherInfo.temp = Math.round(data.main.temp);
      this.weatherInfo.humid = data.main.humidity;
      this.weatherInfo.wind = data.wind.speed;
      this.weatherShow = true;
    },

    getLandmarks: async function (lat, lng) {
      this.landmarksNotFound = false;
      this.landmarksShow = false;
      const keyAPI = "5ae2e3f221c38a28845f05b6010a52c4fc902b08ca31deb9ef521d61";
      const radiusURL = `http://api.opentripmap.com/0.1/ru/places/radius?radius=5000&lon=${lng}&lat=${lat}&rate=2&format=json&limit=5&apikey=${keyAPI}`;
      const response = await fetch(radiusURL);
      const data = await response.json();
      let size = data.length < 5 ? data.length : 5;
      if (data.length <= 0) {
        this.landmarksNotFound = true;
        return;
      }
      for (let i = 0; i < size; ++i) {
        this.landmarksInfo[i].id = data[i].xid;
        this.landmarksInfo[i].name = data[i].name;
        this.landmarksInfo[i].rate = data[i].rate;
      }
      for (let i = 0; i < size; ++i) {
        try {
          const descrURL = `http://api.opentripmap.com/0.1/ru/places/xid/${this.landmarksInfo[i].id}?apikey=${keyAPI}`;
          const descr_resp = await fetch(descrURL);
          const descr_data = await descr_resp.json();
          if ("wikipedia_extracts" in descr_data) {
            this.landmarksInfo[i].descr = descr_data.wikipedia_extracts.text;
          } else {
            continue;
          }
        } catch (e) {
          console.error(e.message);
        }
      }
      this.landmarksShow = true;
    },
  },
};
</script>

<style lang="scss">
@import "./assets/layouts/searching.scss";
@import "./assets/layouts/variants.scss";
@import "./assets/layouts/weather.scss";
@import "./assets/layouts/places-info.scss";
</style>
