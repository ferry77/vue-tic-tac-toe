<template>
  <div class="cell" v-bind:class="{ 'active': active }" @click="handleClick">
    {{ val }}
  </div>
</template>

<script>
export default {
  props: ['player', 'n', 'active'],
  created () {
    // listen event from parent
    this.$on('reset', this.reset)
  },
  data () {
    return {
      val: undefined
    }
  },
  methods: {
    handleClick: function () {
      // if this cell has no value then set it based on current player move
      if (this.val === undefined && this.active) {
        this.val = this.player
        this.$dispatch('cell-clicked', {n: this.n, val: this.val})
      }
    },
    reset () {
      this.val = undefined
    }
  }
}
</script>


<style>
  .cell {
    display: table-cell;
    width: 100px;
    height: 100px;
    border-right: 1px solid #ccc;
    border-bottom: 1px solid #ccc;
    padding: 0;
    margin: 0;
    text-align: center;
    vertical-align: middle;
    font-size: 3em;
    cursor: pointer;
    background-color: #bdbdbd;
  }
  .cell.active {
    background-color: #ffffff;
  }
  .cell.active:hover {
    background: #dddddd;
  }
</style>
