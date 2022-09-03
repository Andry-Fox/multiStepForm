<template>
  <div class="steps__box">
    <button v-if="activeStep -1 >= 0" @click="backStep">Вернутся</button>
    <div class="progress">
      <div class="progress-step"
           :class="{'active':index === activeStep}"
           v-for="( item, index) in formStep"
           :key="index">
        {{ index + 1 }}
      </div>
    </div>
    <button v-if="activeStep +1 < formStep.length" @click="checkValid">Продолжить</button>
    <button v-if="activeStep +1 === formStep.length" @click="sendIt">Отправить</button>
  </div>
</template>

<script>
export default {
  name: "vProgressSteps",
  props: {
    activeStep: Number,
    formStep:{
      type: Array,
      default() {
        return[]
      }
    }
  },
  methods: {
    backStep(){
      this.$emit('backStep')
    },
    checkValid(){
      this.$emit('checkValid')
    },
    sendIt(){
      this.$emit('sendIt')
    },
  }
}
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Noto+Serif:ital,wght@0,400;0,700;1,400;1,700&display=swap');
@mixin flexbox() {
  display: flex;
  justify-content: center;
  align-items: center;
}
.steps__box {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-top: 10px;
  width: 450px;
  height: 55px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 15px 30px rgba(0,0,0,.2),
  0 10px 20px rgb(0 0 0 / 20%);

  button {
    font-family: 'Noto Serif', serif;
    outline: none;
    border: none;
    color: #fff;
    background-color: #5b2d82;
    font-size: 1rem;
    border-radius: 8px;
    height: 70%;
    cursor: pointer;
  }
}
.progress {
  display: flex;
}
.progress-step {
  @include flexbox();
  margin-left: 10px;
  position: relative;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  color: #fff;
  background-color: #5b2d82;
  font-weight: bold;

  &.active {
    background-color: #5b2d82;

    ~ .progress-step {
      color: #555;
      background-color: #ccc;
    }

    ~ .progress-step::before {
      background-color: #ccc;
    }
  }

  &:before {
    content: '';
    position: absolute;
    top: 15px;
    right: 20px;
    width: 20px;
    height: 2px;
    background-color: #5b2d82;
    z-index: 10;
  }

  &:first-child::before {
    display: none;
  }
}
</style>
