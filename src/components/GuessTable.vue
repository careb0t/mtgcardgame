<template>
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
          @mouseover="hoverImage(props.value)"
          @mouseout="clearHoverImage()"
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
            <q-badge
              class="table-badge"
              :style="{ backgroundColor: subtypeBadgeMatch(subtype) }"
            >
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
    <img
      v-for="card in guesses"
      :key="card.name"
      class="card-thumbnail"
      :class="hoveredImage == card ? 'hovered' : ''"
      :src="card.imageUrl"
    />
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
import cardList from "../assets/commander_cards.json";

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
  name: "GuessTable",
  props: ["guesses", "answer"],
  setup(props) {
    const hoveredImage = ref(null);
    return {
      columns,
      hoveredImage,

      nameMatch(name) {
        if (name === props.answer.name) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      colorMatch(colors) {
        let answerColors = props.answer.colorIdentity;
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
          return "#ffff99";
        } else {
          return "#ff6666";
        }
      },

      cmcMatch(cost) {
        if (cost === props.answer.convertedManaCost) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      subtypeMatch(subtypes) {
        let answerTypes = props.answer.subtypes;
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

      subtypeBadgeMatch(subtype) {
        if (props.answer.subtypes.includes(subtype)) {
          return "#006609";
        } else {
          return "#820713";
        }
      },

      powerMatch(power) {
        if (power === props.answer.power) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      toughnessMatch(toughness) {
        if (toughness === props.answer.toughness) {
          return "#66ff66";
        } else {
          return "#ff6666";
        }
      },

      hoverImage(cardName) {
        hoveredImage.value = cardList.find((card) => card.name === cardName);
        console.log(hoveredImage.value);
      },

      clearHoverImage() {
        hoveredImage.value = null;
      },
    };
  },
});
</script>
<style>
.my-table {
  margin: 50px 0 50px 0;
  min-width: 45vw;
}

.q-table__container {
  background-color: transparent;
}

.card-thumbnail {
  position: absolute;
  max-height: 300px;
  bottom: -680px;
  left: 10vw;
  transform: rotate(-25deg);
  transition: all 0.25s linear;
}

.card-thumbnail.hovered {
  bottom: 20px;
  left: 10vw;
  transform: rotate(0deg);
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
</style>
