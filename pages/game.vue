<template>
  <div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>
    <a class="back" @click.prevent="back">&#10148;</a>
    <menu>
      <div class="info">
        <h2>Ходов</h2>
        <p>{{ number }}</p>
      </div>
      <div class="info">
        <h2>Время</h2>
        <p>{{ hours }}:{{ minutes }}:{{ secunds }}</p>
      </div>
    </menu>

    <transition-group name="fifteen-block" class="fifteen-block">
      <button
        v-for="(button, indexButton) in buttons[0]"
        v-bind:key="button"
        v-on:click="change(button, 0, indexButton)"
        :class="{null: button==0}"
      >
        <span v-if="button != 0">{{ button }}</span>
        <span v-else></span>
      </button>

      <button
        v-for="(button, indexButton) in buttons[1]"
        v-bind:key="button"
        v-on:click="change(button, 1, indexButton)"
        :class="{null: button==0}"
      >
        <span v-if="button != 0">{{ button }}</span>
        <span v-else></span>
      </button>

      <button
        v-for="(button, indexButton) in buttons[2]"
        v-bind:key="button"
        v-on:click="change(button, 2, indexButton)"
        :class="{null: button==0}"
      >
        <span v-if="button != 0">{{ button }}</span>
        <span v-else></span>
      </button>

      <button
        v-for="(button, indexButton) in buttons[3]"
        v-bind:key="button"
        v-on:click="change(button, 3, indexButton)"
        :class="{null: button==0}"
      >
        <span v-if="button != 0">{{ button }}</span>
        <span v-else></span>
      </button>
    </transition-group>
  </div>
</template>

<script>
export default {
  data: () => ({
    secunds: 0,
    minutes: 0,
    hours: 0,
    buttons: [
      [1, 2, 3, 4].sort(function() {
        return Math.random() - 0.5;
      }),
      [5, 6, 7, 8].sort(function() {
        return Math.random() - 0.5;
      }),
      [9, 10, 11, 12].sort(function() {
        return Math.random() - 0.5;
      }),
      [13, 14, 15, 0].sort(function() {
        return Math.random() - 0.5;
      })
    ].sort(function() {
      return Math.random() - 0.5;
    }),
    number: 0,
  }),
  mounted () {
    this.interval = setInterval(() => {
      if (this.secunds < 60) this.secunds++
      else if (this.minutes < 60) { 
        this.minutes++
        this.secunds = 0
      }
      else {
        this.hours++
        this.minutes = 0
        this.secunds = 0
      }
    }, 1000);
  },
  methods: {
    change: function(button, indexLine, indexButton) {
      let newButtons = []
      for (let i = 0; i < this.buttons.length; i++) {
        newButtons.push([])
        for (let j = 0; j < this.buttons[i].length; j++) {
          newButtons[i].push(this.buttons[i][j])
        }
      }

      if (indexLine != 0) {
        if (newButtons[indexLine - 1][indexButton] === 0) {
          newButtons[indexLine - 1][indexButton] = button
          newButtons[indexLine][indexButton] = 0
          this.number += 1
        }
      }

      if (indexButton != 0) {
        if (newButtons[indexLine][indexButton - 1] === 0) {
          newButtons[indexLine][indexButton - 1] = button
          newButtons[indexLine][indexButton] = 0
          this.number += 1
        }
      }

      if (indexButton != newButtons[indexLine].length - 1) {
        if (newButtons[indexLine][indexButton + 1] === 0) {
          newButtons[indexLine][indexButton + 1] = button
          newButtons[indexLine][indexButton] = 0
          this.number += 1
        }
      }

      if (indexLine != newButtons.length - 1) {
        if (newButtons[indexLine + 1][indexButton] === 0) {
          newButtons[indexLine + 1][indexButton] = button
          newButtons[indexLine][indexButton] = 0
          this.number += 1
        }
      }

      this.buttons = newButtons

      let endGame = true
      for (let i=0; i<this.buttons.length; i++) {
        for (let j=0; j<this.buttons[i].length; j++) {
          if (this.buttons[i][j] != 0 && this.buttons[i][j] != (i*4)+(j+1)) {
            endGame = false
            break
          }
        }
      }
      if (endGame) alert("Вы выиграли")

    },
    back: function() {
      this.$router.push('/')
    }
  },
  beforeDestroy () {
    clearInterval(this.interval)
  }
};
</script>

<style scoped>
.back{
  font-size: 3rem;
  color:#3F3B70;
  margin-left: 100px;
  margin-top: 20px;
  writing-mode: tb-rl;
  transform:rotate(90deg);
}
.back:hover {
  cursor: pointer;
}

menu {
  display: grid;
  grid-template-columns: 200px 200px;
  justify-content: center;
  text-align: center;
  grid-gap: 15px;
  padding: 0;
  margin-bottom: 30px;
  margin-top: -40px;
}
.info {
  background: #D4D0FB;
  color: #262343;
}
.info h2,
.info p {
  margin: 10px;
}
.info p {
  font-size: 1.5rem;
}

.fifteen-block-move {
  transition: transform 0.5s;
}
.fifteen-block {
  display: grid;
  grid-template-rows: 100px 100px 100px 100px;
  grid-template-columns: 100px 100px 100px 100px;
  justify-content: center;
  grid-gap: 10px;
}

.fifteen-block button {
  background: #3F3B70;
  color: white;
  border: none;
  font-size: 3rem;
  padding: 0;
}
.fifteen-block button:hover {
  cursor: pointer;
}

.fifteen-block .null {
  background: none;
  border: none;
}
</style>
