<template>
  <b-container class="">
    <Navigation />
    <div class="content">
      <Nuxt />
    </div>
  </b-container>
</template>

<script>
import Navigation from "../components/Navigation.vue";

export default {
  components: {
    Navigation,
  },
};
</script>

<style>
body {
  background-color: rgb(255, 254, 253);
  padding: 0 !important;
  margin: 0 !important;
}

.main-container {
  border-radius: 0;
  min-height: 100vh;
  background: rgb(255, 255, 255, 0.9);
}

.btn-ak {
  background-color: #f59b1a;
  font-weight: bold;
}

.btn-ak:hover {
  -webkit-box-shadow: 0px 5px 7px -1px #5e5e5e;
  box-shadow: 0px 5px 7px -1px #5e5e5e;
  background-color: #f58d14;
}

.btn-ak:focus {
  box-shadow: none;
}

.borde {
  border: solid red 1px;
}
</style>
