<script>
import { ref, watch } from 'vue'
import Card from './components/Card.vue'
export default{
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    const userSelected = ref([])
    const status = ref('')

    for (let i= 0; i < 12; i++) {
      cardList.value.push({
          value: 10,
          visible: false,
          position: i,
          matched: false
      })
    }

    const flipCard = payload => {
      cardList.value[payload.position].visible = true

      if (userSelected.value[0]) {
        userSelected.value[1] = payload
      } else {
        userSelected.value[0] = payload
      }
    }

    watch(userSelected, currentValue => {
      if (currentValue.lenght === 2) {
        
        const cardOne = currentValue[0]
        const cardTwo = currentValue[1]

        if (cardOne.faceValue === cardTwo.faceValue) {
          status.value = 'Matched!'

          cardList.value[cardOne.position].matched = true
          cardList.value[cardTwo.position].matched = true

        } else {
          status.value = 'Mismatched!'

          cardList.value[cardOne.position].visible = false
        cardList.value[cardTwo.position].visible = false
        }

      

        userSelected.lenght = 0
      }
      },
      { deep : true }
    )

    return {
      cardList,
      flipCard,
      userSelected,
      status
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
    :matched="card.matched"
    :value="card.value"
    :visible="card.visible"
    :position="card.position"
    @select-card="flipCard"
    />
  </section>
  <h2>{{ status }}</h2>
</template>


<style>
#app {
  font-family: 'Courier New', Courier, monospace;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
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
