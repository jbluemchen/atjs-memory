<script>
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
        const selectCard = () => {
            context.emit('select-card', {
                position: props.position,
                faceValue: props.value,
                matched: props.matched
            })
        }

        return {
            selectCard
        }
    }
}
</script>

<template>
    <div class="card" @click="selectCard">
        <div v-if="visible" class="card-face front">
            <img :src="`/pics/${value}.png`" :alt="value" />
            <!-- Final try to get the audio to work
                <audio :src="`/sounds/${value}.wav`"></audio>   -->
        </div>
        <div v-else class="card-face back">
    </div>
    </div>
</template>

<style>
.card {
  border: 5px solid black;
  position: relative;
  padding: 0px;
}
.card-face {
    width: 100%;
    height: 100%;
    position: absolute;
}
.card-face.front {
    background-color: #fff;
}
.card-face.back {
    background-image: url(../pics/genshin-icon.png);
}
</style>