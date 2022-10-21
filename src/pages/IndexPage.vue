<template>
  <q-page class="flex flex-center vertical-top column beleren">
    <img
      alt="Card of the day"
      src="~assets/card_back.png"
      style="width: 200px"
      class="card"
    />
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
    <q-select
      dark
      rounded
      outlined
      :disable="status !== null"
      transition-show="scale"
      transition-hide="scale"
      popup-content-class="results"
      placeholder="Search for a card"
      color="black"
      v-model="selectedCard"
      use-input
      hide-selected
      fill-input
      @update:model-value="makeGuess(selectedCard)"
      input-debounce="0"
      :options="options"
      @filter="filterFn"
      style="width: 250px"
    >
      <template v-slot:no-option>
        <q-item>
          <q-item-section class="text-white">No cards found.</q-item-section>
        </q-item>
      </template>
    </q-select>
    <div class="my-table">
      <q-table
        title="Your Guesses"
        :rows="guesses"
        :columns="columns"
        row-key="name"
        dark
        table-class="guesses"
        hide-bottom
        flat
      >
        <template #body-cell-name="props">
          <q-td
            :props="props"
            :style="{ backgroundColor: nameMatch(props.value) }"
          >
            {{ props.value }}
          </q-td>
        </template>
        <template #body-cell-colorIdentity="props">
          <q-td
            :props="props"
            :style="{ backgroundColor: colorMatch(props.value) }"
          >
            <template v-for="color in props.value" :key="color">
              <img
                v-if="color == 'R'"
                src="../assets/RedMana.png"
                class="mana-icon"
              />
              <img
                v-if="color == 'B'"
                src="../assets/BlackMana.png"
                class="mana-icon"
              />
              <img
                v-if="color == 'G'"
                src="../assets/GreenMana.png"
                class="mana-icon"
              />
              <img
                v-if="color == 'U'"
                src="../assets/BlueMana.png"
                class="mana-icon"
              />
              <img
                v-if="color == 'W'"
                src="../assets/WhiteMana.png"
                class="mana-icon"
              />
            </template>
          </q-td>
        </template>
        <template #body-cell-subtypes="props">
          <q-td
            :props="props"
            :style="{ backgroundColor: subtypeMatch(props.value) }"
          >
            <template v-for="subtype in props.value" :key="subtype">
              <q-badge class="table-badge" color="black">
                {{ subtype }}
              </q-badge>
            </template>
          </q-td>
        </template>
        <template #body-cell-convertedManaCost="props">
          <q-td
            :props="props"
            :style="{ backgroundColor: cmcMatch(props.value) }"
          >
            {{ props.value }}
          </q-td>
        </template>
        <template #body-cell-power="props">
          <q-td
            :props="props"
            :style="{ backgroundColor: powerMatch(props.value) }"
          >
            {{ props.value }}
          </q-td>
        </template>
        <template #body-cell-toughness="props">
          <q-td
            :props="props"
            :style="{ backgroundColor: toughnessMatch(props.value) }"
          >
            {{ props.value }}
          </q-td>
        </template>
      </q-table>
    </div>
  </q-page>
  <q-icon
    name="help_outline"
    size="md"
    class="help-button"
    color="white"
    @click="toggleHelp()"
  />
  <div
    class="help-screen flex flex-center column beleren"
    :class="{ darken: helpMenuOpen == true }"
    @click="toggleHelp()"
  >
    <h4>Welcome to Tibalt & Tamiyo</h4>
    <div class="rules-card flex flex-center">
      <p>
        Every day a new Commander is chosen at random from the list of all
        Commander format legal cards. The goal of the game is to guess the
        correct Commander.
      </p>
      <p>
        You have five guesses per day to guess the correct card. The energy
        counters represent your remaining guesses. If you make an incorrect
        guess, one will turn <span class="red-help">red</span>.
      </p>
      <img class="help-energy" src="../assets/help1.png" />
      <p>
        Each incorrect guess will give you hints about what your guess shares in
        common with the Commander of the day.
        <span class="red-help">Red</span> means that your guess does not match
        the same category for the answer card at all.
        <span class="yellow-help">Yellow</span> means that your guess partially
        matches the same category for the answer card. You are getting close!
        <span class="green-help">Green</span> means your guess is an exact match
        to the answer card for that category. Well done!
      </p>
      <img class="help-guess" src="../assets/help2.png" />
      <p>
        In the example above, "Brims" Barone, Midway Mobster is an incorrect
        guess. It is revealed that the color identity and subtypes for this card
        are a partial match to the correct Commander, while the toughness is an
        exact match, and the converted mana cost and power are not matches.
      </p>
    </div>
  </div>
  <div
    class="spotlight-bg flex flex-center column beleren"
    :class="{ darken: status === 'win' || status === 'lose' }"
  >
    <h1>You {{ status }}!</h1>
    <h5 @click="status = 'postgame'"><u>Return to results</u></h5>
  </div>
  <div class="tibalt" :class="{ spotlight: status === 'lose' }">
    <img alt="tibalt" src="~assets/tibalt.png" style="" class="tibalt-img" />
    <div class="quote-scroll">
      <span>
        "I'm not a fan of the whole 'let's all get along' thing. I prefer 'let's
        all get along and then stab each other in the back.'"
      </span>
    </div>
  </div>
  <div class="tamiyo" :class="{ spotlight: status === 'win' }">
    <img alt="tamiyo" src="~assets/tamiyo.png" style="" class="tamiyo-img" />
    <div class="quote-scroll">
      <span>
        "I've spent my life studying the past, present, and future. I've never
        quite seen someone as vile as you, Tibalt."
      </span>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
import cardList from "../assets/commander_cards.json";

const cardNames = cardList.map((card) => card.name);

const columns = [
  {
    name: "name",
    required: true,
    label: "Card Name",
    align: "left",
    field: (row) => row.name,
    format: (val) => `${val}`,
    sortable: true,
  },
  {
    name: "colorIdentity",
    align: "center",
    label: "Color Identity",
    field: "colorIdentity",
  },
  {
    name: "convertedManaCost",
    align: "center",
    label: "CMC",
    field: "convertedManaCost",
    sortable: true,
  },
  { name: "subtypes", align: "center", label: "Type", field: "subtypes" },
  {
    name: "power",
    align: "center",
    label: "Power",
    field: "power",
    sortable: true,
  },
  {
    name: "toughness",
    align: "center",
    label: "Toughness",
    field: "toughness",
    sortable: true,
  },
];

export default defineComponent({
  name: "IndexPage",
  setup() {
    const options = ref(cardNames);
    const guesses = ref([]);
    const answer = ref(
      cardList.find((card) => card.name === "Breya, Etherium Shaper")
    );
    const chances = ref(5);
    const status = ref(null);
    const helpMenuOpen = ref(false);
    return {
      selectedCard: ref(null),
      options,
      columns,
      guesses,
      answer,
      chances,
      status,
      helpMenuOpen,

      filterFn(val, update, abort) {
        if (val.length < 2) {
          abort();
          return;
        }

        update(() => {
          const needle = val.toLowerCase();
          options.value = cardNames.filter(
            (v) => v.toLowerCase().indexOf(needle) > -1
          );
        });
      },

      makeGuess(name) {
        const card = cardList.find((card) => card.name === name);

        if (name === answer.value.name) {
          guesses.value.push(card);
          status.value = "win";
        } else if (chances.value === 1) {
          guesses.value.push(card);
          status.value = "lose";
        } else {
          guesses.value.push(card);
          chances.value--;
        }
      },

      nameMatch(name) {
        if (name === answer.value.name) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      colorMatch(colors) {
        let answerColors = answer.value.colorIdentity;
        let matchCount = 0;

        for (let i = 0; i < colors.length; i++) {
          if (answerColors.includes(colors[i])) {
            matchCount++;
          }
        }

        if (
          answerColors.length === colors.length &&
          matchCount === colors.length
        ) {
          console.log("full match");
          return "#66ff66";
        } else if (matchCount > 0) {
          console.log("partial match");
          return "#ffff99";
        } else {
          console.log("no match");
          return "#ff6666";
        }
      },

      cmcMatch(cost) {
        if (cost === answer.value.convertedManaCost) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      subtypeMatch(subtypes) {
        let answerTypes = answer.value.subtypes;
        let matchCount = 0;

        if (subtypes.sort() === answerTypes.sort()) {
          return "#66ff66";
        } else {
          for (let i = 0; i < subtypes.length; i++) {
            if (answerTypes.includes(subtypes[i])) {
              matchCount++;
            }
          }
          if (matchCount > 0) {
            return "#ffff99";
          } else {
            return "#ff6666";
          }
        }
      },

      powerMatch(power) {
        if (power === answer.value.power) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      toughnessMatch(toughness) {
        if (toughness === answer.value.toughness) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      toggleHelp() {
        console.log("toggle help");
        helpMenuOpen.value = !helpMenuOpen.value;
      },
    };
  },
});
</script>
<style>
.q-page {
  justify-content: start !important;
}

.card {
  margin: 10px 0 30px 0;
}

.chance-container {
  margin-bottom: 30px;
}

.energy-counter {
  max-width: 50px;
  margin: 0px 10px;
}

.red-energy {
  filter: invert(18%) sepia(84%) saturate(7087%) hue-rotate(358deg)
    brightness(88%) contrast(131%);
}

.search {
  width: 400px;
  margin: 50px 0 50px 0;
}

.results {
  background-color: rgba(0, 0, 0, 0.6);
}

.my-table {
  margin: 50px 0 50px 0;
}

.q-table__container {
  background-color: transparent !important;
}

.tibalt {
  position: absolute;
  left: 0;
  top: 100px;
  width: 300px;
}

.tibalt-img {
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
  width: 100%;
  margin: 0;
  padding: 0;
}

.tamiyo {
  position: absolute;
  right: 0;
  top: 100px;
  width: 300px;
}

.tamiyo-img {
  width: 100%;
}

.quote-scroll {
  background-color: rgb(241, 255, 176);
  width: 100%;
  padding: 10px 25px;
  margin-top: -5px;
  background-image: url(../assets/card_texture.jpeg);
  background-repeat: repeat;
}

.table-badge {
  margin: 0 3px;
}

.mana-icon {
  width: 20px;
  margin: 0 3px;
}

.noMatch {
  background-color: red;
}

.fullMatch {
  background-color: green;
}

.partialMatch {
  background-color: yellow;
}

.spotlight-bg {
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  z-index: -1;
  color: white;
}

.spotlight {
  z-index: 11;
  width: 400px;
}

.help-button {
  position: fixed;
  left: 10px;
  bottom: 20px;
}

.help-screen {
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  z-index: -1;
  color: white;
  background-color: black;
}

.rules-card {
  width: 80%;
  border-radius: 10px;
  border: 2px solid white;
  padding: 30px;
  font-size: 18px;
}

.green-help {
  color: green;
}

.red-help {
  color: red;
}

.yellow-help {
  color: yellow;
}

.help-energy {
  width: 300px;
  margin-bottom: 30px;
}

.help-guess {
  width: 500px;
  margin-bottom: 30px;
}

.darken {
  background-color: rgba(0, 0, 0, 0.9);
  z-index: 10;
}
</style>
