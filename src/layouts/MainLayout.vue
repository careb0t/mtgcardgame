<template>
  <q-layout view="hHh lpR fFf">
    <q-page-container>
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
html,
body {
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}
</style>
