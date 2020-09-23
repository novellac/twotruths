<template>
  <div id="app">
    <h2 class="text-4xl text-center">Which one is FALSE?</h2>
    <p class="text-center">
      {{ resultMessage }}
    </p>

    <!-- Cards -->
    <div class="flex justify-center">
      <Card
        v-for="fact in facts"
        :key="fact.id"
        :fact="fact"
        :selected="isSelected(fact.id)"
        @calculate="calculateVeracity(fact)"
      />
    </div>

    <!-- Results -->
    <div class="flex justify-center m-8">
      <button
        @click="anotherRoundPlease()"
        class="px-4 py-2 font-semibold underline"
      >
        Do you want to
        <span v-if="playingGame && selectedCards.length > 0">scrap it and</span>
        play another round?
      </button>
    </div>
    <div>
      <p>Total points: {{ totalPoints }}</p>
      <button class="px-1 border-2 rounded-lg" @click="resetTally()">
        Clear points
      </button>
    </div>
  </div>
</template>

<script>
import json from "@/assets/data.json";
import Card from "@/components/Card.vue";

export default {
  components: {
    Card,
  },
  name: "App",
  data() {
    return {
      playingGame: true,
      maxChances: json.gameData.maxChances,
      chances: null,
      resultMessage: null,
      facts: json.facts,
      userMessages: json.userMessages,
      selectedCards: [],
      totalPoints: 0,
    };
  },
  created() {
    this.chances = this.maxChances;
    this.resultMessage = `You have ${this.chances} chances.`;
  },
  methods: {
    isSelected(factId) {
      return this.selectedCards.includes(factId) || !this.playingGame;
    },
    calculateVeracity(guess) {
      this.addToCardsArray(guess.id);
      if (guess.veracity === false) {
        this.resultMessage = `Yup, "${guess.msg}" is a total lie. You win!`;
        this.playingGame = false;
        this.totalPoints += 1;
      } else if (this.chances >= this.maxChances) {
        this.chances -= 1;
        this.resultMessage = `Nope, actually "${guess.msg}" is the truth. You have ${this.chances} more chance(s).`;
      } else {
        this.resultMessage = `Nope, actually "${guess.msg}" is the truth. And you're out of chances. You lose.`;
        this.playingGame = false;
      }
    },
    addToCardsArray(cardId) {
      if (!this.selectedCards.includes(cardId)) {
        this.selectedCards.push(cardId);
      }
    },
    anotherRoundPlease() {
      this.chances = this.maxChances;
      this.playingGame = true;
      this.resultMessage = `${this.userMessages.startOver} ${this.userMessages.chancesAvailable} ${this.maxChances}`;
      this.selectedCards = [];
    },
    resetTally() {
      this.totalPoints = 0;
      this.anotherRoundPlease();
    },
  },
};
</script>
