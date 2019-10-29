<template>
  <div>
    <a class="back" @click.prevent="back">&#10148;</a>
    <menu>
      <div class="info">
        <h2>Ходов</h2>
        <p>{{ number }}</p>
      </div>
      <div class="info">
        <h2>Время</h2>
        <p>{{ hours }}:{{ minutes }}:{{ seconds }}</p>
      </div>
    </menu>

    <transition-group name="fifteen-block" class="fifteen-block">
      <button
        v-for="(button, indexButton) in buttons"
        v-bind:key="button"
        v-on:click="onClick(button, indexButton)"
        :class="{null: button === 0}"
      >
        <span v-if="button != 0">{{ button }}</span>
      </button>
    </transition-group>
  </div>
</template>

<script>
export default {
  data: () => ({
    // время
    seconds: 0,
    minutes: 0,
    hours: 0,
    buttons: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 0].sort(function() {
      return Math.random() - 0.5;
    }),
    // счет
    number: 0,
    // границы поля
    lBorder: [0, 4, 8, 12],
    rBorder: [3, 7, 11, 15],
    vS: 4,
    hS: 1   
  }),
  mounted () {
    this.interval = setInterval(() => {
      if (this.seconds < 60) this.seconds++
      else if (this.minutes < 60) { 
        this.minutes++
        this.seconds = 0
      }
      else {
        this.hours++
        this.minutes = 0
        this.seconds = 0
      }
    }, 1000);
  },
  methods: {
    onClick: function(button, indexButton) {
      let newButtons = this.createNewButtons()
      let indexesOppo = [] // возможные варианты расположения нуля
      
      if (indexButton > this.rBorder[0]) indexesOppo.push(indexButton - this.vS)
      if (indexButton < this.lBorder[3]) indexesOppo.push(indexButton + this.vS)
      if (indexButton > this.lBorder[0] && !this.inter(indexButton, this.lBorder)) indexesOppo.push(indexButton - this.hS)
      if (indexButton < this.rBorder[3] && !this.inter(indexButton, this.rBorder)) indexesOppo.push(indexButton + this.hS)

      for (let index of indexesOppo) {
        if (newButtons[index] === 0) {
          newButtons[index] = button
          newButtons[indexButton] = 0
        }
      }     
      
      this.buttons = newButtons
      if (this.checkEndGame()) alert("Вы выиграли")
    },
    back: function() {
      this.$router.push('/')
    },
    
    createNewButtons: function() {
      let newButtons = []
      for (let i = 0; i < this.buttons.length; i++) {
        newButtons.push(this.buttons[i])
      }
      return newButtons      
    },
    checkEndGame: function() {
      let endGame = true
      for (let i=0; i<this.buttons.length - 1; i++) {
        if (this.buttons[i] != i+1) endGame = false;
      }
      return endGame
    },
    inter: function(el, arr1) {
      if (arr1.indexOf(el) != -1) return true
      return false
    }
  },
  beforeDestroy () {
    clearInterval(this.interval)
  }
};
</script>

<style scoped>
.back {
  font-size: 3rem;
  color: #3f3b70;
  margin-left: 100px;
  margin-top: 20px;
  writing-mode: tb-rl;
  transform: rotate(90deg);
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
  background: #d4d0fb;
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
