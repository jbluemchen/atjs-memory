<script>
import { computed, ref, watch } from 'vue'
import _ from 'lodash'
import Card from './components/Card.vue'
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

      cardList.value.forEach((card, index) => {
      card.position = index;
        card.visible = false;
        card.matched = false;
    });

  
    }

    const cardItems = [1, 2, 3, 4, 5, 6]

    cardItems.forEach(item => {
      cardList.value.push({
        value: item,
        variant: 1,
        visible: false,
        position: null,
        matched: false
      })
      cardList.value.push({
        value: item,
        variant: 2,
        visible: false,
        position: null,
        matched: false
      })
    })

    shuffleCards()


    cardList.value.forEach((card, index) => {
    card.position = index;
    });
  

    const flipCard = payload => {
      cardList.value[payload.position].visible = true

      if (payload.matched === true) {
        return
      } else {
        if (userSelected.value[0]) {
        if (userSelected.value[0].position === 
        payload.position && 
        userSelected.value[0].faceValue === 
        payload.faceValue) {
          return
        } else {
          userSelected.value[1] = payload
        }
        userSelected.value[1] = payload
      } else {
        userSelected.value[0] = payload
      }
      }
    }


    
    watch(
      userSelected,
      currentValue => {
        if (currentValue.length === 2) {
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
          userSelected.value.length = 0
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
  <p></p>
  <button class="playAgain" @click="playAgain">Play again</button>
  <h2>{{ status }}</h2>
</template>


<style>
#app {
  font-family: 'Courier New', Courier, monospace;
  text-align: center;
  color: #111;
  margin-top: 60px;
}

.game-board{
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}

.playAgain {
  font-weight: bold;
  align-items: center;
  background-color: hotpink;
  border: 2px solid #111;
  border-radius: 8px;
  box-sizing: border-box;
  color: #111;
  font-family: 'Courier New', Courier, monospace;
  font-size: 16px;
  height: 48px;
  position: relative;
  text-align: center;
}
</style>
