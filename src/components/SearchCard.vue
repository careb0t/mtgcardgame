<template>
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
    v-model="newCard"
    use-input
    hide-selected
    fill-input
    @update:model-value="makeGuess(newCard)"
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
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
import cardList from "../assets/commander_cards.json";
const cardNames = cardList.map((card) => card.name);

export default defineComponent({
  name: "SearchCard",
  props: ["status", "answer", "chances"],
  setup(props) {
    const newCard = ref(null);
    const options = ref(cardNames);
    return {
      newCard,
      options,

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
        let imageId = card.identifiers.scryfallId;
        let url = `https://api.scryfall.com/cards/${imageId}?format=image&version=normal`;
        card.imageUrl = url;
        this.$emit("makeGuess", card);
        if (name === props.answer.name) {
          this.$emit("updateStatus", "win");
          this.$emit("updateChances", props.chances - 1);
        } else if (props.chances === 1) {
          this.$emit("updateStatus", "lose");
          this.$emit("updateChances", props.chances - 1);
        } else {
          this.$emit("updateChances", props.chances - 1);
        }
      },
    };
  },
});
</script>
<style>
.search {
  width: 400px;
  margin: 50px 0 50px 0;
}

.results {
  background-color: rgba(0, 0, 0, 0.6);
}
</style>
