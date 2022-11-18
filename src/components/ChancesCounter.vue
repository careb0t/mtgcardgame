<template>
  <div class="chance-container">
    <img
      src="../assets/Energy.png"
      class="energy-counter"
      :class="{ 'red-energy': chances <= 4 }"
    />
    <img
      src="../assets/Energy.png"
      class="energy-counter"
      :class="{ 'red-energy': chances <= 3 }"
    />
    <img
      src="../assets/Energy.png"
      class="energy-counter"
      :class="{ 'red-energy': chances <= 2 }"
    />
    <img
      src="../assets/Energy.png"
      class="energy-counter"
      :class="{ 'red-energy': chances <= 1 }"
    />
    <img
      src="../assets/Energy.png"
      class="energy-counter"
      :class="{ 'red-energy': chances <= 0 }"
    />
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";

export default defineComponent({
  name: "ChancesCounter",
  props: ["chances"],
  setup(props) {
    return {};
  },
});
</script>
<style>
.chance-container {
  margin-bottom: 30px;
  max-height: 15vh;
}

.energy-counter {
  max-width: 50px;
  margin: 0px 10px;
}

.energy-counter.red-energy {
  filter: invert(18%) sepia(84%) saturate(7087%) hue-rotate(358deg)
    brightness(88%) contrast(131%);
}
</style>
