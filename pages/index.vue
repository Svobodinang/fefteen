<template>
  <div class="main-section">
    <div>
      <h1>Игра "Пятнашки"</h1>
      <button @click.prevent="startGame">Играть</button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    startGame() {
      this.$router.push('/game')
    }
  }
}
</script>

<style scoped>
  .main-section {
    display: flex;
    justify-content: center;
    margin-top: 10rem;
  }
  h1 {
    color: #3F3B70;
  }
  button {
    width: 100%;
    height: 3rem;
    border: none;
    background: #7D75DF;
    font-size: 1.5rem;
    color: white;
  }
  button:hover {
    cursor: pointer;
  }
</style>

