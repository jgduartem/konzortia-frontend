<template>
  <div class="flex bg-dark text-grey-4" style="height: 100vh">
    <div id="name">
      <q-intersection>
        <transition appear enter-active-class="animated fadeInLeft">
          <h1>Jose Duarte</h1>
        </transition>
      </q-intersection>
      <q-intersection>
        <transition appear enter-active-class="animated fadeInLeft">
          <h6 class="row col-12 justify-center">
            Konzortia Capital Developer Test.
          </h6>
        </transition>
      </q-intersection>
    </div>
  </div>
</template>

<script>
export default {
  name: "PageIndex",
};
</script>

<style>
@media (max-height: 700px) {
  #name {
    margin-top: 30%;
    margin-left: 5%;
  }
}
@media (min-height: 730px) {
  #name {
    margin-top: 35vh;
    margin-left: 5%;
  }
}
@media (min-height: 850px) {
  #name {
    margin-top: 70vh;
    margin-left: 5%;
  }
}
@media (min-height: 1024) {
  #name {
    margin-top: 90%;
    margin-left: 5%;
  }
}
@media (min-height: 1240px) {
  #name {
    margin-top: 60%;
    margin-left: 5%;
  }
}
@media (min-height: 1800px) {
  #name {
    margin-top: 40%;
    margin-left: 5%;
  }
}
</style>
