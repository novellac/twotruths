<template>
  <div id="app">
    <h2>Click the one that is FALSE</h2>

    <!-- Cards -->
    <div v-if="playingGame">
      <div v-for="(fact, index) in facts" :key="index">
        <button @click="calculateVeracity(fact)">
          {{ fact.msg }}
        </button>
      </div>
    </div>

    <!-- Results -->
    <div>
      <p v-if="playingGame">Remaining chances: {{ chances }}</p>
      <p>{{ resultMessage }}</p>
      <button v-if="!playingGame" @click="resetGame()">
        Play again?
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
export default {
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
        },
        {
          msg: "I have a dog",
          veracity: true,
        },
        {
          msg: "I have a computer",
          veracity: true,
        },
      ],
    };
  },
  created() {
    this.chances = this.maxChances;
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
    },
  },
};
</script>
