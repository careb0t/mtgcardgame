<template>
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
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";

export default defineComponent({
  name: "HelpPage",
  setup() {
    const helpMenuOpen = ref(false);
    return {
      helpMenuOpen,

      toggleHelp() {
        helpMenuOpen.value = !helpMenuOpen.value;
      },
    };
  },
});
</script>
<style>
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
