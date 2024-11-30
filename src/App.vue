<script>
import AppCard from './components/AppCard.vue';
import GameControls from './components/GameControls.vue';
const CARDS = [
  // Each card appears twice in the array for pairs
  { id: 1, name: 'Vue', image: 'https://upload.wikimedia.org/wikipedia/commons/9/95/Vue.js_Logo_2.svg', isFlipped: false, isMatched: false },
  { id: 2, name: 'React', image: 'https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg', isFlipped: false, isMatched: false },
  { id: 3, name: 'Angular', image: 'https://upload.wikimedia.org/wikipedia/commons/c/cf/Angular_full_color_logo.svg', isFlipped: false, isMatched: false },
  { id: 4, name: 'Vue', image: 'https://upload.wikimedia.org/wikipedia/commons/9/95/Vue.js_Logo_2.svg', isFlipped: false, isMatched: false },
  { id: 5, name: 'React', image: 'https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg', isFlipped: false, isMatched: false },
  { id: 6, name: 'Angular', image: 'https://upload.wikimedia.org/wikipedia/commons/c/cf/Angular_full_color_logo.svg', isFlipped: false, isMatched: false },
]

export default {
  name: 'MemoryGame',
  components: {
    AppCard,
    GameControls
  },
  data() {
    return {
      isPlaying: false,
      cards: structuredClone(CARDS),
      matchedCards: [],
      previousCardName: '',
      hasWon: false
    };
  },
  watch: {
    matchedCards: {
      handler(newState) {
        console.log('watcher' , newState)
        if (newState.length === 3) {
          this.hasWon = true;
          this.resetState()
        }
      },
      deep:true
    }
  },
  methods: {
    resetState() {
      this.isPlaying = false;
      this.cards = structuredClone(CARDS);
      this.matchedCards = [];
      this.previousCardName = '';
    },
    onStart() {
      this.isPlaying = true;
      this.hasWon = false;
      this.cards = structuredClone(CARDS)
    },
 
    onFlip(cardId) {
      if (!this.isPlaying)
        return;

      const numberOfFlipped = this.cards.filter(card => card.isFlipped).length;
      if (numberOfFlipped === 2) {
        this.cards.forEach(card => card.isFlipped = false);
      }

      const selectedCard = this.cards.find(x => x.id === cardId);
      if (!selectedCard)
        return;

      selectedCard.isFlipped = !selectedCard.isFlipped;

      if (!this.previousCardName) {
        this.previousCardName = selectedCard.name;
        return;
      }
      if (this.previousCardName === selectedCard.name) {
        this.matchedCards.push(selectedCard.name);
      }
      this.previousCardName = null;
    },

  },
};
</script>

<template>
  <div class="memory-game-container">
    <h1>Memory Game</h1>
    <GameControls :is-playing="isPlaying" @start="onStart" @stop="resetState" :hasWon="hasWon"/>

    <div class="game-board">
      <AppCard v-for="card in cards" :id="card.id" :key="card.id" :url="card.image" :alt="card.name"
        :is-flipped="card.isFlipped || matchedCards.includes(card.name)" @flip="onFlip" />
    </div>
  </div>
</template>

<style scoped>
.memory-game-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-top: 2rem;
}
</style>
