<template>
  <div id="main">
    <div class="container">
      <div class="row gx-5">
        <div class="col-4">
          <div class="search">
            <form class="search__location" @submit="getCoords">
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
                placeholder="What City?"
                autocomplete="off"
              />
              <button
                type="button"
                class="btn btn-outline-success"
                @click="getCoords"
              >
                Find
              </button>
            </form>
          </div>
        </div>

        <div class="col">
          <div class="variants">
            <div class="variants__title">Варианты мест</div>
            <ul class="variants__buttons">
              <button
                type="button"
                class="btn btn-outline-primary"
                v-for="(button, id) in buttonsList"
                :key="id"
              >
                {{ button.name }}
              </button>
            </ul>
            <ul class="variants__coords">
              <li class="coord" v-for="(coor, id) in coordsList" :key="id">
                {{ coor.lat }}<br />{{ coor.lon }}
              </li>
            </ul>
          </div>
        </div>

        <div class="col-12">
          <div class="places">
            <div class="places__info">
              <div class="name">Место 1</div>
              <div class="descr">Здесь могла быть ваша реклама</div>
              <div class="weather">Температура:</div>
            </div>
            <div class="places__info">
              <div class="name">Место 2</div>
              <div class="descr">Здесь могла быть ваша реклама</div>
              <div class="weather">Температура:</div>
            </div>
            <div class="places__info">
              <div class="name">Место 3</div>
              <div class="descr">Здесь могла быть ваша реклама</div>
              <div class="weather">Температура:</div>
            </div>
            <div class="places__info">
              <div class="name">Место 4</div>
              <div class="descr">Здесь могла быть ваша реклама</div>
              <div class="weather">Температура:</div>
            </div>
            <div class="places__info">
              <div class="name">Место 5</div>
              <div class="descr">Здесь могла быть ваша реклама</div>
              <div class="weather">Температура:</div>
            </div>
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
      buttonsList: [
        { id: 0, name: "???" },
        { id: 1, name: "???" },
        { id: 2, name: "???" },
        { id: 3, name: "???" },
        { id: 4, name: "???" },
      ],
      coordsList: [
        { id: 0, lat: "???", lon: "???" },
        { id: 1, lat: "???", lon: "???" },
        { id: 2, lat: "???", lon: "???" },
        { id: 3, lat: "???", lon: "???" },
        { id: 4, lat: "???", lon: "???" },
      ],
    };
  },
  methods: {
    getCoords: async function () {
      const keyAPI = "fc66e5ec568a2bd3c45c86a9b418c2c2";
      console.log(keyAPI);
      console.log(this.placeSearch);
      const coordsURL =
        "http://api.openweathermap.org/data/2.5/weather?q=${this.placeSearch}&appid=fc66e5ec568a2bd3c45c86a9b418c2c2";
      const response = await fetch(coordsURL);
      const data = await response.json();
      console.log(data);
      this.placeSearch = "";
    },
  },
};
</script>

<style lang="scss">
@import "./assets/layouts/searching.scss";
@import "./assets/layouts/variants.scss";
@import "./assets/layouts/places-info.scss";
</style>
