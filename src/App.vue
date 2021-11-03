<template>
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
              placeholder="What City?"
              autocomplete="off"
            />
            <button type="button" class="btn btn-outline-success">Find</button>
          </form>
        </div>
      </div>
      <div class="col">
        <div class="variants">
          <div class="variants__title">Варианты мест</div>
          <div class="variants__buttons">
            <button type="button" class="btn btn-outline-primary">But1</button>
            <button type="button" class="btn btn-outline-primary">But2</button>
            <button type="button" class="btn btn-outline-primary">But3</button>
            <button type="button" class="btn btn-outline-primary">But4</button>
            <button type="button" class="btn btn-outline-primary">But5</button>
          </div>
          <div class="variants__coords">
            <div class="coord">lat:500 lon:700</div>
            <div class="coord">lat:500 lon:700</div>
            <div class="coord">lat:500 lon:700</div>
            <div class="coord">lat:500 lon:700</div>
            <div class="coord">lat:500 lon:700</div>
          </div>
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
</template>

<script>
export default {
  name: "App",
};
</script>

<style lang="scss">
@import "./assets/layouts/searching.scss";
@import "./assets/layouts/variants.scss";
@import "./assets/layouts/places-info.scss";
</style>
