<script setup>
import { defineProps, ref, watch } from 'vue';

const userScore = ref(0);
const computerScore = ref(0);
const prop = defineProps(['resultat']);

// Uppdaterar poängen
watch(prop, () => {
  if (prop.resultat.winner === 'user') {
    userScore.value++;
  } else if (prop.resultat.winner === 'computer') {
    computerScore.value++;
  }
});

// Funktion för att återställa 
function GameReset() {
  location.reload();
}
</script>

<template>
  <!-- Visar poäng och knapp för att återställa -->
  <div v-if="userScore || computerScore" class="Score">
    <span id="userScore">
      <span :class="userScore > computerScore ? 'leader score' : 'score'">{{ userScore }}</span>
      <p>User</p>
    </span>

    <span id="computerScore">
      <span :class="userScore < computerScore ? 'leader score' : 'score'">{{ computerScore }}</span>
      <p>Computer</p>
    </span>
  </div>
  <div class="Reset">
    <ul>
      <!-- Knapp för att återställa poängen -->
      <li @click="GameReset">Reset points</li>
    </ul>
  </div>
</template>

<style scoped>
.Score {
  margin: auto;
  padding: 0.5em;
  width: 50%;
  display: flex;
  justify-content: center;
}

.Reset {
  padding-left: 10%;
}

ul {
  display: flex;
  justify-content: center;
  padding-top: 20%;
}

li {
  list-style-type: none;
  margin: 0.5em;
  background-color: white;
  padding: 1em;
  border: 2px solid black;
  border-radius: 0.5em;
  display: inline-block;
}

.score {
  background-color: aqua;
  font-size: 3em;
  border: 2px solid black;
  width: 3em;
  height: 1.6em;
  display: inline-block;
  text-align: center;
  margin: 0.2em;
}

.leader {
  color: rgb(168, 255, 7);
  font-size: 5em;
}
</style>
