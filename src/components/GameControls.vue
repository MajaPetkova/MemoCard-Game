<script>
const GAME_TIME = 65;
let interval;

export default {
    props: {
        isPlaying: {
            type: Boolean,
            required: true
        }
    },
    emits: ['start', 'stop'],
    data() {
        return {
            secondsLeft: 0,
            hasLost: false
        }
    },
    methods: {
        handleStart() {
            this.$emit('start');
            this.secondsLeft = GAME_TIME;
            interval = setInterval(() => {
                if (this.secondsLeft > 1) {
                    this.secondsLeft -= 1;
                }else{
                  this.hasLost =true;
                  this.stopGame()
                }
            }, 1000)
        },
        stopGame(){
            clearInterval(interval);
            this.secondsLeft=0;
            this.$emit('stop')
          
        }
    }
}

</script>
<template>
    <section class="gameControls">
        <div class="btns">
            <button @click="handleStart" :disabled="isPlaying">Start</button>
            <button @click="stopGame" :disabled="!isPlaying">Stop</button>
           
        </div>
    </section>
    <h3 v-if="secondsLeft">Seconds Left: {{ secondsLeft}}</h3>
    <p v-if="hasLost">Sorry, you lost the game!</p>
</template>
<style scoped>
.gameControls {
    margin: 2rem 0;
}

.btns {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
}
</style>