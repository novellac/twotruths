<template>
  <div id="app">
    <h2 class="text-4xl text-center">Which one is FALSE?</h2>
    <p class="text-center">
      {{ resultMessage }}
    </p>

    <!-- Cards -->
    <div class="flex justify-center">
      <Card
        v-for="(fact, index) in facts"
        :key="index"
        :fact="fact"
        @calculate="calculateVeracity(fact)"
      >
      </Card>
    </div>

    <!-- Results -->
    <div class="flex justify-center m-8">
      <button @click="resetGame()" class="px-4 py-2 font-semibold underline">
        Do you want to <span v-if="playingGame">scrap it and</span> play again?
      </button>
    </div>
  </div>
</template>

<script>
import json from "@/assets/data/facts.json";
import Card from "@/components/Card.vue";

export default {
  components: {
    Card,
  },
  name: "App",
  data() {
    return {
      playingGame: true,
      maxChances: 2,
      chances: null,
      resultMessage: null,
      facts: json.facts,
    };
  },
  created() {
    this.chances = this.maxChances;
    this.resultMessage = `You have ${this.chances} chances.`;
  },
  methods: {
    calculateVeracity(guess) {
      if (guess.veracity === false) {
        this.resultMessage = `Yup, "${guess.msg}" is a total lie. You win!`;
        this.playingGame = false;
      } else if (this.chances >= this.maxChances) {
        this.chances -= 1;
        this.resultMessage = `Nope, actually "${guess.msg}" is the truth. You have ${this.chances} more chance(s).`;
      } else {
        this.resultMessage = `Nope, actually "${guess.msg}" is the truth. And you're out of chances. You lose.`;
        this.playingGame = false;
      }
    },
    resetGame() {
      this.chances = this.maxChances;
      this.playingGame = true;
      this.resultMessage = null;
    },
  },
};
</script>
