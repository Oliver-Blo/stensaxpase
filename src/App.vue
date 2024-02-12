<script setup>
import { ref } from 'vue';
import GameAlternatives from './components/GameAlternatives.vue';
import GameScore from './components/GameScore.vue';
import GameResult from './components/GameResult.vue';

const alternative = ref(""); 
const compAlternative = ref(''); 
const winnerInfo = ref(''); 
const winner = ref(''); 
const round = ref(0); 

// Funktion för att sätta information om vinnaren
function setWinnerInfo(_winner) {
  winner.value = _winner;
  round.value++;

  // Säger vem som vann
  if (_winner === "draw") {
    winnerInfo.value = "Det är lika";
  } else if (_winner === "user") {
    winnerInfo.value = "Du vann";
  } else {
    winnerInfo.value = "Du förlorade";
  }
}

// Sätter användarens valda alternativ
function setUserAlternative(alt) {
  alternative.value = alt;
}

// Sätter datorns valda alternativ
function setComputerAlternative(alt) {
  compAlternative.value = alt;
}
</script>

<template>
  <h1>Sten Sax Påse</h1><br>

  <GameAlternatives @user-choice="setUserAlternative" @computer-choice="setComputerAlternative" @winner="setWinnerInfo"/>
  <GameScore :user-alternative="alternative" :computer-alternative="compAlternative" :winner-info="winnerInfo"/>
  <GameResult :resultat="{round, winner}" />
</template>

<style scoped>
h1 {
  text-align: center;
  min-width: 10px;
}
</style>
