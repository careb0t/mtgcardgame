<template>
  <img
    alt="Card of the day"
    :src="getArt()"
    style="width: 350px"
    class="card"
  />
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";

export default defineComponent({
  name: "CardArt",
  props: ["answer", "chances", "status"],
  setup(props) {
    return {
      getArt() {
        let imageId = props.answer.identifiers.scryfallId;
        let url = `https://api.scryfall.com/cards/${imageId}?format=image&version=art_crop`;

        return url;
      },
    };
  },
});
</script>
<style>
.card {
  margin: 10px 0 30px 0;
  width: 500px;
}
</style>
