<template>
  <div id="app">
    <h2 class="text-4xl text-center">Which one is FALSE?</h2>
    <p v-if="playingGame" class="text-center">
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
    <div class="flex justify-end">
      <button
        @click="resetGame()"
        class="px-4 py-2 font-semibold text-white rounded bg-gradient-to-r from-teal-400 to-blue-500 hover:from-pink-500 hover:to-orange-500"
      >
        Do you want to <span v-if="playingGame">scrap it and</span> play again?
      </button>
    </div>

    <div>
      <h2>Roadmap</h2>
      <ul>
        <li>Clean this up. I'm sure I could improve the logic.</li>
        <li>
          Make it visual. There should be cards! And they should flip! And be
          pretty!
        </li>
        <li>Put the data in an external file.</li>
        <li>Make it possible to play more than one round.</li>
        <li>Make it a tutorial, because why not.</li>
        <li>Keep track of points earned over all rounds of play.</li>
      </ul>
    </div>
  </div>
</template>

<script>
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
      facts: [
        {
          msg: "I have a cat",
          veracity: false,
          emoji: "🐈",
        },
        {
          msg: "I have a dog",
          veracity: true,
          emoji: "🐶",
        },
        {
          msg: "I have a computer",
          veracity: true,
          emoji: "💻",
        },
      ],
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
