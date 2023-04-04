<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleStart($event)" />
  <play-screen v-if="statusMatch === 'match'" :cardContext="settings.cardContext" @onResult="onResultScreen($event)"/>
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain = "onAgain"/>
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import PlayScreen from "./components/PlayScreen.vue"
import ResultScreen from "./components/ResultScreen"
import {shuffled} from "./utils/array"
export default {
  name: 'App',
  components: {
    MainScreen,
    PlayScreen,
    ResultScreen
  },

  data(){
    return{
      settings: {
        totalBlocks: 0,
        cardContext: [],
        startedAt: null,
      },
      statusMatch: 'default',
      timer: Number,
    }
  },

  methods: {
    onHandleStart(config){
      console.log(config);
      this.settings.totalBlocks = config.totalBlocks;
      const firstCards = Array.from({length: this.settings.totalBlocks /2}, (_,i) => i+1);
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.settings.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));
      this.settings.startedAt = new Date().getTime();
      this.statusMatch = 'match';
    },
    onResultScreen(count){
      if(count == this.settings.totalBlocks/2)
        setTimeout(()=>{
          this.statusMatch = 'result';
        }, 800);
      this.timer = new Date().getTime() - this.settings.startedAt;
    },
    onAgain(){
      this.statusMatch = 'default';
    }
  }
}
</script>

