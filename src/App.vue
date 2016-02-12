<template>
  <div id="app">
    <h1>Tic Tac Toe</h1>
    <h2 v-show="winner === undefined">Player {{ player }} turn</h2>
    <h2 v-show="winner !== undefined">Winner is Player {{ winner }}</h2>
    <button @click="reset">RESET</button>
    <div class="board">
      <div class="row">
        <cell n="1" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
        <cell n="2" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
        <cell n="3" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
      </div>
      <div class="row">
        <cell n="4" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
        <cell n="5" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
        <cell n="6" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
      </div>
      <div class="row">
        <cell n="7" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
        <cell n="8" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
        <cell n="9" :player="player" :winning-condition="winningCondition" @cell-clicked="handleChildCellClicked"></cell>
      </div>
    </div>
  </div>
</template>

<script>
import Cell from './components/Cell'
const winningConditions = ['123', '456', '789', '147', '258', '369', '159', '357']

export default {
  components: {
    Cell
  },
  data () {
    return {
      player: 0, // this boolean value represent player turn 0 or 1
      winner: undefined,
      winningCondition: '',
      boardCell: {} // this state will holds and map each cell value
    }
  },
  methods: {
    handleChildCellClicked: function (data) {
      if (this.winner === undefined) {
        // single source of truth from child cell value
        this.boardCell[data.n] = data.val
        if (!this.hasWinningCondition()) {
          this.switchPlayer()
        } else {
          this.winner = this.player
        }
      }
    },
    switchPlayer () {
      this.player = Number(!this.player)
    },
    hasWinningCondition () {
      var self = this
      self.winningCondition = winningConditions.filter(function (condition) {
        return (Number(self.boardCell[condition[0]]) === Number(self.boardCell[condition[1]]) && Number(self.boardCell[condition[1]]) === Number(self.boardCell[condition[2]]))
      }).pop()
      return self.winningCondition
    },
    reset () {
      this.winner = undefined
      this.player = 0
      this.boardCell = {}
      this.$broadcast('reset')
    }
  }
}
</script>

<style>
  #app {
    text-align: center;
  }
  .board {
    display: table;
    margin: 0 auto;
  }
  .board .row {
    display: table-row;
  }
  .board .row .cell:last-child {
    border-right: none;
  }
  .board .row:last-child .cell {
    border-bottom: none;
  }
  button {
    margin-bottom: 30px;
  }
</style>
