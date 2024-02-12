<script setup>
import { ref, defineEmits } from 'vue';

// Emittar user-choice och computer-choice
const emit = defineEmits(['user-choice', 'computer-choice']);

// Skriver alternativen
const alternatives = ref(['Scissors', 'Paper', 'Rock', 'Lizard', 'Spock']);

// Visar när användaren väljer ett alternativ och tar bort klassen vald
function alternativeChosen(e) {
  let buttons = document.getElementsByClassName('button');
  for (let b of buttons) {
    b.classList.remove('vald');
  }

  // Hämtar valda alternativet och lägger till klassen vald
  let alternative = e.target.innerText;
  e.target.classList.add('vald');
  emit('user-choice', alternative);

  //Visar datorns val
  computerAction();
}

// Funktion för att simulera datorns val
function computerAction() {
  // Slumpar ett alternativ för datorn
  let compAlternative = alternatives.value[Math.floor(Math.random() * alternatives.value.length)];

  // Hämtar alla knappar och tar bort 'computerChoice'
  let buttons = document.getElementsByClassName('button');
  for (let b of buttons) {
    b.classList.remove('computerChoice');
    // Lägger till 'computerChoice'
    if (b.innerText === compAlternative) {
      b.classList.add('computerChoice');
    }
  }

  // Skickar datorns val till förälderkomponenten
  emit('computer-choice', compAlternative);

  // Avgör vinnaren baserat på valen
  determineWinner();
}

// Funktion för att avgöra vinnaren
function determineWinner() {
  let userButton, computerButton;

  // Hämtar alla knappar och identifierar användarens och datorns val
  let buttons = document.getElementsByClassName('button');
  for (let b of buttons) {
    if (b.classList.contains('vald')) {
      userButton = b.innerText;
    }
    if (b.classList.contains('computerChoice')) {
      computerButton = b.innerText;
    }
  }

  // Hittar användarens och datorns val
  let computerIndex = alternatives.value.indexOf(computerButton);
  let userIndex = alternatives.value.indexOf(userButton);

  // Skickar vidare baserat på vinnaren
  if (computerButton === userButton) {
    emit('winner', 'draw');
  } else if (computerIndex % 2 === userIndex % 2) {
    emit('winner', computerIndex > userIndex ? 'computer' : 'user');
  } else {
    emit('winner', computerIndex < userIndex ? 'computer' : 'user');
  }
}
</script>

<template>
  <ul>
    <li v-for="alt in alternatives" @click="alternativeChosen" class="button" :key="alt">{{ alt }}</li>
  </ul><br>
</template>

<style scoped>
ul {
  display: flex;
  justify-content: center;
  padding-left: 0;
}

li {
  list-style-type: none;
  margin: 0.5em;
  background-color: white;
  padding: 1em;
  border: 2px solid black;
  border-radius: 0.5em;
}

.vald {
  background-color: rgb(65, 255, 7);
}

.computerChoice {
  border: 0.2em dashed black;
}
@media only screen and (max-width:1000px) {
  ul {
  display: flex;
  justify-content: center;
  padding-left: 0;
  font-size:0.7em;
}
}
</style>
