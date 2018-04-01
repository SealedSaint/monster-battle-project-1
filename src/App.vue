<template>

<div id="app">
  <div class="combatants-section main-section" v-if="gameState === gameStates.playing">
    <div class="player-details combatant-details">
      <div class="name">{{ player.name.toUpperCase() }}</div>
      <div class="life-bar-container">
        <div class="life-bar"></div>
        <span class="life-total">{{ player.life }}</span>
      </div>
    </div>
    <div class="monster-details combatant-details">
      <div class="name">{{ monster.name.toUpperCase() }}</div>
      <div class="life-bar-container">
        <div class="life-bar"></div>
        <span class="life-total">{{ monster.life }}</span>
      </div>
    </div>
  </div>
  <div class="buttons-section main-section">
    <div class="new-game-buttons buttons-container" v-if="gameState === gameStates.new">
      <button @click="gameState = gameStates.playing">Start New Game</button>
    </div>
    <div class="playing-game-buttons buttons-container" v-else>
      <button>Attack</button>
      <button>Heal</button>
      <button @click="gameState = gameStates.new">Surrender</button>
    </div>
  </div>
  <div class="game-log-section main-section" v-if="logs.length">

  </div>
</div>

</template>


<!-- SCRIPT -->

<script>
const GAME_STATES = {
  new: 'NEW',
  playing: 'PLAYING',
};

export default {
  name: 'app',
  data: () => ({
    player: {
      name: 'Player 1',
      life: 100,
    },
    monster: {
      name: 'Monster',
      life: 100,
    },
    gameStates: GAME_STATES,
    gameState: GAME_STATES.playing,
    logs: [],
  }),
};
</script>


<!-- Style -->

<style>

.main-section {
  margin: 15px;
  padding: 15px;
  border: 1px solid lightgrey;
}

.combatants-section {
  display: flex;
  justify-content: space-around;
}

.combatant-details {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.combatant-details > .name {
  font-size: 20px;
}

.life-bar-container {
  position: relative;
  width: 300px;
  height: 50px;
  background: lightgrey;
}

.life-bar {
  width: 100%;
  height: 100%;
  background: lightgreen;
}

.life-total {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-weight: bold;
}

.buttons-container {
  display: flex;
  justify-content: center;
}

.buttons-container > button {
  margin: 10px;
}

</style>
