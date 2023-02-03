<template>
  <div id="app">
    <mapVue></mapVue>
  </div>
</template>

<script>
import mapVue from "./components/map.vue";
export default {
  name: "App",
  components: { mapVue },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
</style>
