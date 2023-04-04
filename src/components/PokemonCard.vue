<template>
  <div class="card" :class="{'disabled': isDisabled}" :style="{
    height: `${(821 - 16*4 ) / Math.sqrt(cardContext.length)-16}px`,
    width: `${((821 - 16*4 ) / Math.sqrt(cardContext.length)-16) * 3 / 4}px`,
    perspective: `${((821 - 16*4 ) / Math.sqrt(cardContext.length)-16) * 3 / 4 *2}px`
  }">
    <div class="card_inner" :class="{'isflipped' : isFlip}" @click="onToggleFlipCard">
      <div class="card_face card_face_front">
        <div class="card_content" :style="{
          backgroundSize: `${((821 - 16*4 ) / Math.sqrt(cardContext.length)-16) * 3 / 4 /3}px ${((821 - 16*4 ) / Math.sqrt(cardContext.length)-16) * 3 / 4 /3}px`
        }"></div>
      </div>
      <div class="card_face card_face_back">
        <div class="card_content" :style="{ backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})` }"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    card: {
      type: [String, Number, Object, Array],
    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    cardContext: {
      type: Array,
    }
  },
  data(){
    return{
      isDisabled: false,
      isFlip: false,
    }
  },
  methods: {
    onToggleFlipCard(){
      if(this.isDisabled) return false;
      this.isFlip = !this.isFlip;
      if(this.isFlip){
        this.$emit("onFlip", this.card);
      }
    },
    onFlipBackCard(){
      this.isFlip = false;
    },
    onDisabledCard(){
      this.isDisabled = true;
    }
  }
}
</script>

<style lang="css" scoped>
.card{
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card_inner{
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card_inner.isflipped {
  transform: rotateY(-180deg);
}

.card_face{
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card_face_front .card_content{
  background: url("../assets/images/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

.card_face_back{
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card_face_back .card_content{
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
}

.card.disabled .card_inner{
  cursor: default;
}

</style>