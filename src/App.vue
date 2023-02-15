<script>
import Card from './components/card.vue'
export default{
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = []

    for (let i= 0; i < 12; i++) {
      cardList.push(i)
    }

    return {
      cardList
    }
    
  }
}
</script>

<template>
  <h1>Sound memory</h1>
  <section class="game-board">
    <Card 
    v-for="(card, index) in cardList"
    :key="`card-${index}`"
    :value="card"
    />
  </section>
</template>


<style>
#app {
  font-family: 'Courier New', Courier, monospace;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.card {
  border: 5px solid #ccc
}
.game-board{
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
