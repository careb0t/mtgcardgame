<template>
  <q-page class="flex flex-center column beleren">
    <img
      alt="Card of the day"
      src="~assets/card_back.png"
      style="width: 200px"
      class="card"
    />
    <q-select
      dark
      rounded
      outlined
      transition-show="scale"
      transition-hide="scale"
      popup-content-class="results"
      placeholder="Search for a card"
      color="black"
      v-model="model"
      use-input
      hide-selected
      fill-input
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
        :rows="rows"
        :columns="columns"
        row-key="name"
        dark
        table-class="guesses"
        hide-bottom
      />
    </div>
  </q-page>
  <div class="tibalt">
    <img alt="tibalt" src="~assets/tibalt.png" style="" class="tibalt-img" />
    <div class="quote-scroll">
      <span>
        "I'm not a fan of the whole 'let's all get along' thing. I prefer 'let's
        all get along and then stab each other in the back.'"
      </span>
    </div>
  </div>
  <div class="tamiyo">
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
import { defineComponent, ref } from "vue";

const cardList = [
  "Oath of Teferi",
  "Teferi, Hero of Dominaria",
  "Teferi, Mage of Zhalfir",
  "Teferi, Master of Time",
  "Teferi, Temporal Archmage",
  "Teferi, Time Raveler",
  "Teferi, Timebender",
  "Teferi, Timeless Voyager",
  "Teferi, Who Slows the Sunset",
  "Teferi's Ageless Insight",
  "Teferi's Care",
  "Teferi's Curse",
  "Teferi's Drake",
  "Teferi's Honor Guard",
  "Teferi's Imp",
  "Teferi's Isle",
  "Teferi's Moat",
  "Teferi's Protection",
  "Teferi's Protege",
  "Teferi's Realm",
  "Teferi's Response",
  "Teferi's Sentinel",
  "Teferi's Time Twist",
  "Teferi's Tutelage",
  "Teferi's Veil",
  "Teferi's Wavecaster",
];

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
  { name: "color", align: "center", label: "Color ID", field: "color" },
  { name: "cmc", align: "center", label: "CMC", field: "cmc" },
  { name: "type", align: "center", label: "Type", field: "type" },
  { name: "power", align: "center", label: "Power", field: "power" },
  {
    name: "toughness",
    align: "center",
    label: "Toughness",
    field: "toughness",
  },
  { name: "rarity", align: "center", label: "Rarity", field: "rarity" },
  { name: "set", align: "center", label: "Set", field: "set" },
];

const rows = [
  {
    name: "Oath of Teferi",
    color: "WU",
    cmc: "5",
    type: "Legendary Enchantment",
    power: "NA",
    toughness: "NA",
    rarity: "Rare",
    set: "Dominaria",
  },
  {
    name: "Teferi, Hero of Dominaria",
    color: "WU",
    cmc: "3",
    type: "Legendary Planeswalker - Teferi",
    power: "3",
    toughness: "3",
    rarity: "Mythic Rare",
    set: "Dominaria",
  },
  {
    name: "Tiballt, the Fiend-Blooded",
    color: "BR",
    cmc: "4",
    type: "Legendary Planeswalker - Tibalt",
    power: "3",
    toughness: "3",
    rarity: "Mythic Rare",
    set: "Dominaria",
  },
  {
    name: "Siege Rhino",
    color: "WGB",
    cmc: "4",
    type: "Creature - Rhino",
    power: "4",
    toughness: "5",
    rarity: "Rare",
    set: "Khans of Tarkir",
  },
];

export default defineComponent({
  name: "IndexPage",
  setup() {
    const options = ref(cardList);
    return {
      model: ref(null),
      options,
      columns,
      rows,

      filterFn(val, update, abort) {
        if (val.length < 2) {
          abort();
          return;
        }

        update(() => {
          const needle = val.toLowerCase();
          options.value = cardList.filter(
            (v) => v.toLowerCase().indexOf(needle) > -1
          );
        });
      },
    };
  },
});
</script>
<style>
.card {
  margin: 50px 0 50px 0;
}

.search {
  width: 400px;
  margin: 50px 0 50px 0;
}

.results {
  background-color: rgba(0, 0, 0, 0.6);
  /*font-family: "mtg";*/
}

.my-table {
  margin: 50px 0 50px 0;
  background-color: rgba(0, 0, 0, 0.6) !important;
}

.tibalt {
  position: absolute;
  left: 0;
  top: 100px;
}

.tibalt-img {
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
  width: 300px;
  margin: 0;
  padding: 0;
}

.tamiyo {
  position: absolute;
  right: 0;
  top: 100px;
}

.tamiyo-img {
  width: 300px;
}

.quote-scroll {
  background-color: rgb(241, 255, 176);
  width: 300px;
  padding: 10px 25px;
  margin-top: -5px;
}
</style>
