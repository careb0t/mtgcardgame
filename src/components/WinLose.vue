<template>
  <div
    class="spotlight-bg flex flex-center column beleren"
    :class="{
      darken: status === 'win' || status === 'lose',
      hidden: status === 'postgame',
    }"
  >
    <SmokeCover />
    <img :src="getImage(answer)" class="wl-card" />
    <h1>You {{ status }}!</h1>
    <h4 @click="copyScore()"><u>Share your score</u></h4>
    <h5 @click="returnResults()">Return to results</h5>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
import { useQuasar } from "quasar";
import SmokeCover from "./SmokeCover.vue";

export default defineComponent({
  name: "WinLose",
  props: ["status", "answer", "chances"],
  components: { SmokeCover },
  setup(props) {
    const notify = useQuasar().notify;
    return {
      returnResults() {
        this.$emit("updateStatus", "postgame");
      },

      getImage() {
        let imageId = props.answer.identifiers.scryfallId;
        let url = `https://api.scryfall.com/cards/${imageId}?format=image&version=border_crop`;
        return url;
      },

      copyScore() {
        navigator.clipboard.writeText(
          `Tibalt & Tamiyo\n${this.createScore()}\nhttps://commandle.io`
        );
        notify({
          message: "Score copied to clipboard!",
          color: "positive",
          position: "bottom",
          timeout: 2000,
        });
      },

      createScore() {
        if (props.chances == 4) {
          return "🧙‍♂️🎴 🟩⬛⬛⬛⬛";
        } else if (props.chances == 3) {
          return "🧙‍♂️🎴 🟥🟩⬛⬛⬛";
        } else if (props.chances == 2) {
          return "🧙‍♂️🎴 🟥🟥🟩⬛⬛";
        } else if (props.chances == 1) {
          return "🧙‍♂️🎴 🟥🟥🟥🟩⬛";
        } else if (props.chances == 0 && props.status == "win") {
          return "🧙‍♂️🎴 🟥🟥🟥🟥🟩";
        } else if (props.chances == 0 && props.status == "lose") {
          return "🧙‍♂️🎴 🟥🟥🟥🟥🟥";
        }
      },
    };
  },
});
</script>
<style>
h4,
h5 {
  cursor: pointer;
  margin: 15px 0;
}

.spotlight-bg {
  position: fixed;
  display: none;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  z-index: -1;
  color: white;
  transition: all 0.5s linear;
  opacity: 0;
}

.darken {
  background-color: rgba(0, 0, 0, 0.99);
  display: flex;
  z-index: 10;
  opacity: 1;
}

.hidden {
  opacity: 0;
}

.wl-card {
  max-height: 45vh;
}
</style>
