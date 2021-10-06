<template>
  <div class="">
    <div
      class="row align-items-rigth justify-content-rigth justify-content-md-between"
    >
      <div class="col-6 my-2 col-md-3 text-rigth">
        <img src="~/static/logo_THOT1.png" class="w-100" alt="THOT" />
        <!-- <h1 class="display-4 text-center">THOT</h1> -->
      </div>
      <div class="col-12 col-md-9">
        <b-nav tabs justified>
          <b-nav-item to="/" exact exact-active-class="active">
            <b-icon icon=""></b-icon>
            Control
          </b-nav-item>
          <b-nav-item to="/movements-log" exact exact-active-class="active">
            <b-icon icon=""></b-icon>
            Registro
          </b-nav-item>
        </b-nav>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Navigation",
};
</script>

<style>
.nav {
  white-space: nowrap;
}
.nav-link {
  color: rgb(0, 0, 0);
  border-bottom-color: rgb(0, 0, 0) !important;
  font-weight: bold;
}
.nav-link:hover {
  color: rgb(255, 255, 255);
  background-color: rgb(0, 0, 0);
  transition: all 0s;
}

.nuxt-link-active {
  background-color: rgb(161, 161, 161) !important;
  color: #2d2d2d !important;
  border-color: #fff !important;
  border-bottom-color: rgb(0, 0, 0) !important;
}
</style>
