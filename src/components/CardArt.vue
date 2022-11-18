<template>
  <div class="card" :style="{ backgroundImage: `url(${getArt()})` }">
    <div class="box" :class="isHidden(1) ? 'bHidden' : ''"></div>
    <div class="box" :class="isHidden(2) ? 'bHidden' : ''"></div>
    <div class="box" :class="isHidden(3) ? 'bHidden' : ''"></div>
    <div class="box" :class="isHidden(4) ? 'bHidden' : ''"></div>
    <div class="box" :class="isHidden(5) ? 'bHidden' : ''"></div>
    <div class="box" :class="isHidden(6) ? 'bHidden' : ''"></div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";

export default defineComponent({
  name: "CardArt",
  props: ["answer", "chances", "status", "hiddenTiles"],

  setup(props) {
    return {
      getArt() {
        let imageId = props.answer.identifiers.scryfallId;
        let url = `https://api.scryfall.com/cards/${imageId}?format=image&version=art_crop`;

        return url;
      },

      isHidden(num) {
        if (props.hiddenTiles.includes(num)) {
          return true;
        } else {
          return false;
        }
      },
    };
  },
});
</script>
<style>
.card {
  margin: 10px 0 30px 0;
  height: 229px;
  width: 312px;
  max-width: 85vw;
  max-height: 30vh;
  background-size: contain;
  background-repeat: no-repeat;
  display: flex;
  flex-wrap: wrap;
  box-shadow: 10px 10px 30px 0px rgba(0, 0, 0, 1);
}

.box {
  flex: 1 33%;
  background-color: black;
  opacity: 1;
}

.bHidden {
  opacity: 0;
  transition: all 2s linear;
}
</style>
