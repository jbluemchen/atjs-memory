<script>

import { computed } from 'vue'

export default {
    props: {
        matched: {
            type: Boolean,
            default: false
        },
        position: {
            type: Number,
            required: true
        },
        value: {
            type: String,
            required: true
        },
        visible: {
            type: Boolean,
            default: false
        }
    },
    
    setup(props, context) {
        const flippedCard = computed(() => {
            if (props.visible) {
                return 'card-flipped'
            }
        })

        const selectCard = () => {
            context.emit('select-card', {
                position: props.position,
                faceValue: props.value,
                matched: props.matched
            })
        }

        return {
            flippedCard,
            selectCard
        }
    }
}
</script>

<template>
    <div class="card" :class="flippedCard"
    @click="selectCard">
        <div class="card-face front">
            <img :src="`/pics/${value}.png`" class="pictures" :alt="value" />
        </div>
        <!--last try audio
            <audio :src="`/sounds/${value}.wav`"></audio>
        -->
        <div class="card-face back">
    </div>
    </div>
</template>

<style>
.card {
  border: 5px solid lightblue;
  position: relative;
  padding: 0px;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
}
.card.card-flipped {
    transform: rotateY(180deg);
}
.card-face {
    width: 100%;
    height: 100%;
    position: absolute;
    backface-visibility: hidden;
}
.card-face.front {
    background-color: #fff;
    transform: rotateY(180deg);
}
.card-face.back {
    background-image: url(../pics/genshin-logo.png);
}
</style>