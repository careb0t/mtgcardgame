<template>
  <SmokeCover :class="{ hidden: introFinished() }" />
  <q-page
    class="content flex flex-center vertical-top column beleren"
    :class="{
      introFinished: timer <= 1,
    }"
    :style="timer == 0 ? 'background-size: cover;' : ''"
  >
    <q-toolbar :class="{ chidden: !introFinished() }" class="comp">
      <q-toolbar-title class="beleren title">
        Tibalt and Tamiyo
      </q-toolbar-title>
    </q-toolbar>
    <CardArt
      :answer="answer"
      :chances="chances"
      :status="status"
      :hiddenTiles="hiddenTiles"
      :class="{ chidden: !introFinished() }"
      class="comp"
    />
    <ChancesCounter
      :chances="chances"
      :class="{ chidden: !introFinished() }"
      class="comp"
    />
    <SearchCard
      :status="status"
      :answer="answer"
      :guesses="guesses"
      :chances="chances"
      :hiddenTiles="hiddenTiles"
      @makeGuess="makeG($event)"
      @updateStatus="updateS($event)"
      @updateChances="updateC($event)"
      @updateHiddenTiles="updateT($event)"
      :class="{ chidden: !introFinished() }"
      class="comp"
    />
    <GuessTable
      :guesses="guesses"
      :answer="answer"
      :class="{ chidden: !introFinished() }"
      class="comp"
    />
  </q-page>
  <HelpPage :class="{ chidden: !introFinished() }" class="comp" />
  <WinLose
    @updateStatus="updateS($event)"
    :status="status"
    :answer="answer"
    :chances="chances"
    :class="{ chidden: !introFinished() }"
    class="comp"
  />
</template>

<script>
import axios from "src/boot/axios";
import { defineComponent, ref, onMounted } from "vue";
import cardList from "../assets/commander_cards.json";

import CardArt from "src/components/CardArt.vue";
import ChancesCounter from "src/components/ChancesCounter.vue";
import SearchCard from "src/components/SearchCard.vue";
import GuessTable from "src/components/GuessTable.vue";
import HelpPage from "src/components/HelpPage.vue";
import WinLose from "src/components/WinLose.vue";
import SmokeCover from "src/components/SmokeCover.vue";

export default defineComponent({
  name: "IndexPage",
  components: {
    CardArt,
    ChancesCounter,
    SearchCard,
    GuessTable,
    HelpPage,
    WinLose,
    SmokeCover,
  },
  setup() {
    onMounted(() => {
      let interval = setInterval(() => {
        if (timer.value === 0) {
          clearInterval(interval);
        } else {
          timer.value--;
        }
      }, 1000);
    });

    const guesses = ref([]);
    const answer = ref(
      cardList.find((card) => card.name === "Lord Xander, the Collector")
    );
    const chances = ref(5);
    const status = ref(null);
    const timer = ref(2);
    const hiddenTiles = ref([]);

    return {
      guesses,
      answer,
      chances,
      status,
      timer,
      hiddenTiles,

      makeG(card) {
        guesses.value.push(card);
      },

      updateC(num) {
        chances.value = num;
      },

      updateS(str) {
        status.value = str;

        hiddenTiles.value = [1, 2, 3, 4, 5, 6];
      },

      updateT(num) {
        hiddenTiles.value.push(num);
      },

      introFinished() {
        if (timer.value === 0) {
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
.q-page {
  justify-content: start !important;
}

.title {
  color: white;
  font-size: 24px;
}

.spotlight {
  z-index: 11;
  width: 400px;
}

.darken {
  background-color: rgba(0, 0, 0, 0.9);
  z-index: 10;
}

.content {
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  background-image: url(../assets/mountain.jpg);
  background-repeat: no-repeat;
  background-size: 400%;
  background-position: center;
  transition: all 1s linear;
  filter: brightness(0.4);
}

.content.introFinished {
  background-size: 100%;
  filter: brightness(1);
}

.content > .comp.chidden {
  opacity: 0;
  transition: all 2s linear;
}

.content > .comp {
  opacity: 1;
  transition: all 2s linear;
}
</style>
