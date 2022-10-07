<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="text-white transparent">
      <q-toolbar>
        <q-toolbar-title class="beleren"> Tibalt and Tamiyo </q-toolbar-title>
        <q-space />
        <span class="subtitle beleren">Classic</span>
        <span class="subtitle beleren">Commanders</span>
      </q-toolbar>
    </q-header>

    <q-page-container class="bg-image">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";

const linksList = [
  //objects go here
];

export default defineComponent({
  name: "MainLayout",

  components: {
    //components go here
  },

  setup() {
    return {};
  },
});
</script>
<style>
.bg-image {
  background-image: url(../assets/mountain.jpg);
  background-repeat: no-repeat;
  background-size: cover;
}

.subtitle {
  margin-left: 50px;
}
</style>
