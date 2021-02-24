<template>
  <main>
    <wired-card class="fade" v-show="!init">
      <h1>Potluck</h1>
      <p>
        Random decisions, <wired-link href="https://hackernoon.com/how-does-javascripts-math-random-generate-random-numbers-ef0de6a20131#:~:text=For%20starters%2C%20it's%20not%20really%20random&text=random()%20doesn't%20really%20generate%20a%20random%20number.&text=If%20you're%20using%20math,PRNGs%20are%20better%20than%20others.">not-so-random</wired-link> decisions,
        either way, let's make a choice!
      </p>
      <wired-button @click="start">
        Get Started
      </wired-button>
    </wired-card>

    <wired-card class="fade" v-show="init">
      <h2>Potluck</h2>
      <div class="fade" v-show="!loading">
        <wired-input ref="input" v-on:keyup.enter="onEnter" placeholder="Enter an item..."></wired-input>
        <wired-button v-show="!loading" @click="run" v-if="items.length > 0">
          Run
        </wired-button>
        <hr/>
        <table v-show="!loading">
          <tr v-for="(item, index) in items">
            <td>
              {{ item }}
            </td>
            <td>
              <wired-icon-button class="delete" @click="remove(index)">x</wired-icon-button>
            </td>
          </tr>
        </table>
      </div>
      <div class="fade" v-show="loading">
        <wired-spinner spinning></wired-spinner>
        <h3>Virtually handpicking your result now...</h3>
      </div>
    </wired-card>

    <wired-dialog class="dialog" :open="done">
      <h3>
        The winner is: <span>{{ selected }}</span>
      </h3>
      <div>
        <wired-button @click="reset()">Ok</wired-button>
      </div>
    </wired-dialog>
  </main>
</template>

<script>
import 'wired-elements'

export default {
  data() {
    return {
      init: false,
      loading: false,
      done: false,
      items: [],
      selected: null
    }
  },
  methods: {
    start() {
      this.init = true
    },
    onEnter() {
      let input = this.$refs.input.value
      if (input !== '' && input !== null) {
        this.items.push(input)
        this.$refs.input.value = null
      }
    },
    remove(index) {
      this.items.splice(index, 1)
    },
    run() {
      this.loading = true
      let self = this
      window.setTimeout(function(){
        self.selected = self.items[Math.floor((Math.random() * self.items.length))]
        self.done = true
        self.loading = false
      }, 4000)
    },
    reset() {
      this.done = false
    }
  }
}
</script>

<style>
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .dialog div {
    text-align: center;
  }
  main {
    max-width: 400px;
    margin:auto
  }
  table {
    width: 100%;
  }
  td:first-child {
    text-align: left;
  }
  td {
    text-align: right;
  }
  .fade {
    animation: fadein 1s
  }
  @keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
  }
</style>
