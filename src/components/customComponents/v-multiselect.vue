<template>
  <div class="multi__select__body">
    <div class="multi__select__box">
      <div
        v-for="(option,index) in multiSelectItem"
        :key="index"
        class="body"
      >
        <span class="item">{{ option.value }}</span>
        <img class="button" src="@/image/cross-svgrepo-com.svg">
      </div>
    </div>
    <button class="button__open__option" @click="optionRemove">opn</button>
  </div>
  <div class="multi__select__option__box" :class="animationOptionBox" v-if="optionListIsVisible === true">
    <div v-for="(option,index) in multiOptions"
         :key="index"
         @click="checkIsIn(option)"
         class="option"
    >
      {{ option.value }}
    </div>
  </div>
</template>

<script>
export default {
  name: "v-multiselect",
  data() {
    return {
      optionListIsVisible: false,
      animationOptionBox: '',
      multiSelectItem: [],
    }
  },
  props:{
    multiOptions: {
      type: Array,
      default () {
        return []
      }
    }
  },
  methods: {
    checkIsIn(option){
      let isIn;
      isIn = this.multiSelectItem.some(item => item.value === option.value)
      if (!isIn){
        this.multiSelectItem.push(option)
      }else{
        let filter = this.multiSelectItem.filter(item => item.value !== option.value)
        this.multiSelectItem = filter
      }
      console.log(this.multiSelectItem)
    },
    optionRemove() {
      if (this.optionListIsVisible === false) {
        this.optionListIsVisible = true
        this.animationOptionBox = 'optionOpen';
        setTimeout(() => {
          this.animationOptionBox = '';
        }, 1000);
        document.addEventListener("click.self", this.optionClose, true)
      } else {
        this.optionClose()
      }
    },
    optionClose(){
      document.removeEventListener("click", this.optionClose, true)
      this.animationOptionBox = 'optionClose';
      setTimeout(() => {
        this.optionListIsVisible = false
      }, 800);
      setTimeout(() => {
        this.animationOptionBox = '';
      }, 1000);
    },
  }
}
</script>

<style lang="scss" scoped>
  .multi__select__body{
    display: flex;
    margin: 10px;
    min-height: 40px;
    box-sizing: border-box;
    border: 4px solid #760ec8;
    border-radius: 15px;
    background-color: #760ec8;

    .button__open__option{
      padding: 4px 5px;
      background-color: #ffffff;
      border-radius: 10px;
      border: none;
    }
  }
  .multi__select__box{
    width: 250px;
    min-height: 40px;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    flex-grow: 1;

    .body{
      display: flex;
      align-items: center;
      margin: 5px;
      background-color: white;
      padding: 5px;
      border-radius: 10px;
    }
    .item{

    }
    .button{
      color: white;
      width: 14px;
      height: 14px;
      padding: 4px;
      background-color: #760ec8;
      border-radius: 10px;
      border: none;
      margin-left: 8px;
    }
  }
  .multi__select__option__box{
    min-width: 150px;
    position: fixed;
    top: 130px;
    right: -30px;
    padding: 10px;
    box-sizing: border-box;
    border-radius: 15px;
    background-color: #5d1695;
    display: flex;
    flex-direction: column;
    border: 1px solid white;

    .option {
      border: solid 2px #ffffff;
      margin: 4px;
      border-radius: 25px;
      color: white;
      font-family: "Noto Serif", serif;
      font-size: 15px;
      padding: 0px 12px;
      font-weight: 600;
      &:hover {
        background-color: #c0a1db;
        color: #5d1695;
      }
    }
  }
  .optionOpen {
    animation: optionOpen .9s ease-in-out;
  }
  .optionClose {
    animation: optionClose .9s ease-in-out;
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
