<template>
  <div class="select">
    <div class="select-container">
      <span class="item" @click="clickSelect">{{selected_value}} </span>
      <img class="icon_button" :class="animationCross" src="@/image/cross-svgrepo-com.svg" v-if="selected_value" @click="selectClean" >
      <img class="icon" :class="animationArrow" src="@/image/resize-svgrepo-com.svg" @click="clickSelect">
    </div>
    <div class="option-container" :class="animationOptionBox" v-if="show === true">
      <div class="option"
           @click="clickOption(option)"
           v-for="option in options"
           :key="option.id"
      >
        {{ option.value }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "vueSelect",
  props: {
    options: {
      type: Array,
      default () {
        return []
      }
    }
  },
  data() {
    return {
      selected_value: "Ваше образование:",
      show: false,
      animationArrow: '',
      animationOptionBox: '',
      animationCross: '',
    }
  },
  methods: {
    clickSelect() {
      if (this.show === false) {
        this.show = true
        this.animationArrow = 'active';
        setTimeout(() => {
          this.animationArrow = '';
        }, 1000);
        this.animationOptionBox = 'optionOpen';
        setTimeout(() => {
          this.animationOptionBox = '';
        }, 1000);
        document.addEventListener("click", this.optionClose, true)
      } else {
        this.optionClose()
      }
    },
    clickOption(option) {
      this.selected_value = option.value
      this.optionClose()
    },
    optionClose(){
      document.removeEventListener("click", this.optionClose, true)
      this.animationOptionBox = 'optionClose';
      setTimeout(() => {
        this.show = false
      }, 800);
      setTimeout(() => {
        this.animationOptionBox = '';
      }, 1000);
    },
    selectClean() {
      this.selected_value = "Ваше образование:"
      this.animationCross = 'click';
      setTimeout(() => {
        this.animationCross = '';
      }, 600);
    },
  }
}
</script>

<style lang="scss" scoped>

  .select{
    min-width: 400px;
  }
  .select-container {
    margin: 10px;
    height: 40px;

    box-sizing: border-box;
    border: solid 3px #ab83ce;
    border-radius: 25px;
    background-color: #760ec8;

    display: flex;
    align-items: center;

    &:hover {
      background-color: #ab83ce;
      border: solid 3px #760ec8;
    }

    .item{
      margin: 0 10px 0 15px;

      color: white;
      font-family: 'Noto Serif', serif;
      font-size: 20px;

      flex-grow: 1;
    }

    .icon_button {
      width: 16px;
      margin: 1px 1px;
      padding: 4px 5px;

      &:hover {
        width: 22px;
        transition: 0.4s;
      }
    }

    .icon {
      width: 26px;
      margin: 0 -1px;
      padding: 4px 5px;

      background-color: #760ec8;
      border-radius: 25px;
      transform: rotate(180deg);
    }
  }
  .option-container {
    min-width: 150px;
    position: fixed;
    top: 125px;
    right: 170px;

    padding: 10px;

    box-sizing: border-box;
    border: solid 3px #ab83ce;
    border-radius: 25px;
    background-color: #760ec8;

    display: flex;
    flex-direction: column;

    .option {
      border: solid 3px #760ec8;
      padding: 0px 8px;
      color: white;
      font-family: 'Noto Serif', serif;
      font-size: 15px;
      &:hover {
        background-color: #ab83ce;
        border: solid 3px #ffffff;
        border-radius: 25px;
        color: #760ec8;
      }
    }
  }
  .active {
    animation: active .9s ease-in-out;
  }
  .click {
    animation: click .5s ease-in-out;
  }
  .optionOpen {
    animation: optionOpen .9s ease-in-out;
  }
  .optionClose {
    animation: optionClose .9s ease-in-out;
  }

  @keyframes active {
    0% {
      transform:translateY(3px) rotate(180deg);
    }
    80% {
      transform:translateY(-2px) rotate(180deg);
    }
    100% {
      transform:translateY(0px) rotate(180deg);
    }
  }
  @keyframes click {
    0% {
      transform: rotate(180deg);
    }
    100% {
      transform: rotate(0deg);
    }
  }
  @keyframes optionOpen {
    0% {
      opacity: 0;
      transform: translateY(100px) ;
    }
    100% {
      opacity: 1;
      transform: translateY(0px) ;
    }
  }
  @keyframes optionClose {
    0% {
      opacity: 1;
      transform: translateY(0px) ;
    }
    100% {
      opacity: 0;
      transform: translateY(100px) ;
    }
  }
</style>
