<template>
  <div class="card" :style="{ backgroundImage: `url(${getArt()})` }">
    <div
      class="box b1"
      :class="isHidden(1) ? 'bHidden' : ''"
      :style="{ backgroundImage: `url(${getArt()})` }"
    ></div>
    <div
      class="box b2"
      :class="isHidden(2) ? 'bHidden' : ''"
      :style="{ backgroundImage: `url(${getArt()})` }"
    ></div>
    <div
      class="box b3"
      :class="isHidden(3) ? 'bHidden' : ''"
      :style="{ backgroundImage: `url(${getArt()})` }"
    ></div>
    <div
      class="box b4"
      :class="isHidden(4) ? 'bHidden' : ''"
      :style="{ backgroundImage: `url(${getArt()})` }"
    ></div>
    <div
      class="box b5"
      :class="isHidden(5) ? 'bHidden' : ''"
      :style="{ backgroundImage: `url(${getArt()})` }"
    ></div>
    <div
      class="box b6"
      :class="isHidden(6) ? 'bHidden' : ''"
      :style="{ backgroundImage: `url(${getArt()})` }"
    ></div>
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
  opacity: 1;
  filter: blur(4px);
  background-size: 312px 229px;
  background-repeat: no-repeat;
}

.b1 {
  background-position: 0 0;
}

.b2 {
  background-position: 33% 0;
}

.b3 {
  background-position: 66% 0;
}

.b4 {
  background-position: 0 50%;
}

.b5 {
  background-position: 33% 50%;
}

.b6 {
  background-position: 66% 50%;
}

.bHidden {
  opacity: 0;
  transition: all 2s linear;
}
</style>
