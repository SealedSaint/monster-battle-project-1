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
      <button @click="startNewGame">Start New Game</button>
    </div>
    <div class="playing-game-buttons buttons-container" v-else>
      <button @click="attack">Attack</button>
      <button @click="heal">Heal</button>
      <button @click="endGame">Surrender</button>
    </div>
  </div>
  <div class="game-log-section main-section" v-if="gameState === gameStates.playing && logs.length">
    <div class="log" :class="[log.source.toLowerCase(), log.action.toLowerCase()]" v-for="log in reversedLogs">{{ log.message }}</div>
  </div>
</div>

</template>


<!-- SCRIPT -->

<script>
const SOURCES = {
  player: 'Player',
  monster: 'Monster',
};

const ACTIONS = {
  attack: 'ATTACK',
  heal: 'HEAL',
};

class GameLog {
  constructor(source, action, value) {
    this.source = source;
    this.action = action;
    this.value = value;

    // set the message
    switch (action) {
      case ACTIONS.attack:
        this.message = `The ${source} attacked for ${value}.`
        break;
      case ACTIONS.heal:
        this.message = `The ${source} healed for ${value}.`
        break;
      default:
        this.message = `Unknown action action: ${action}.`
    }
  }
}

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
    gameState: GAME_STATES.new,
    logs: [],
  }),
  computed: {
    reversedLogs() {
      return [...this.logs].reverse();
    },
  },
  methods: {
    startNewGame() {
      this.gameState = GAME_STATES.playing;
      this.player.life = 100;
      this.monster.life = 100;
      this.logs = [];
    },
    endGame() {
      this.gameState = GAME_STATES.new;
    },
    getMonsterAttack() { return Math.round(Math.random() * 10); },
    getPlayerAttack() { return Math.round(Math.random() * 12); },
    getPlayerHeal() { return Math.round(Math.random() * 15); },
    attack() {
      const playerAttack = this.getPlayerAttack();
      const monsterAttack = this.getMonsterAttack();

      this.player.life -= monsterAttack;
      this.monster.life -= playerAttack;

      this.logs.push(new GameLog(SOURCES.player, ACTIONS.attack, playerAttack));
      this.logs.push(new GameLog(SOURCES.monster, ACTIONS.attack, monsterAttack));
    },
    heal() {
      const playerHeal = this.getPlayerHeal();
      const monsterAttack = this.getMonsterAttack();

      this.player.life += playerHeal;
      this.player.life -= monsterAttack;

      this.logs.push(new GameLog(SOURCES.player, ACTIONS.heal, playerHeal));
      this.logs.push(new GameLog(SOURCES.monster, ACTIONS.attack, monsterAttack));
    },
  },
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

.log {
  padding: 5px;
  margin: 5px;
  background: lightgrey;
  text-align: center;
}

.log.player {
  background: lightblue;
}

.log.monster {
  background: pink;
}

.log.heal {
  background: lightgreen;
}

</style>
