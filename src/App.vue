<template>
  <div id="app">
    <!-- <mapVue></mapVue> -->
    <threeBasic></threeBasic>
  </div>
</template>

<script>
import mapVue from "./components/map.vue";
import threeBasic from "./components/threeBasic.vue";
export default {
  name: "App",
  components: { mapVue, threeBasic },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
</style>
