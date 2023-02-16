<script>
import { computed, ref, watch } from 'vue'
import _ from 'lodash'
import card from './components/Card.vue'
export default{
  name: 'App',
  components: {
    Card
  },

  setup() {
    const cardList = ref([])
    const userSelected = ref([])
    const status = computed(() => {
      if (remainingPairs.value === 0) {
        return 'You win'
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`
      }
    })
    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter
      (card => card.matched === false).length

      return remainingCards / 2
    })

    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value)
    }

    const playAgain = () => {
      shuffleCards()

      cardList.value = cardList.value.map(card, index => {
        return {
          ...card,
          matched: false,
          position: index,
          visible: false
        }
      })
    }

    const cardItems = [1, 2, 3, 4, 5, 6]

    cardItems.forEach(item => {
      cardList.value.push({
          value: item,
          visible: false,
          position: null,
          matched: false
      })

      cardList.value.push({
          value: item,
          visible: false,
          position: null,
          matched: false
      })
    })

    cardList.value = cardList.value.map(card, index => {
        return {
          ...card,
          position: index
        }
      })
  

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
        
          cardList.value[cardOne.position].matched = true
          cardList.value[cardTwo.position].matched = true

        } else {

          setTimeout(() => {

            cardList.value[cardOne.position].visible = false
            cardList.value[cardTwo.position].visible = false

        }, 2000)
          
        }

      

        userSelected.value.lenght = 0
      }
      },
      { deep: true }
    )

    return {
      cardList,
      flipCard,
      userSelected,
      status,
      shuffleCards,
      playAgain
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
  <button @click="playAgain">Play again</button>
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
