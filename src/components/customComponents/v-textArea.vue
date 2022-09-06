<template>
  <div v-for="(input,index) in textarea"
       :key="index"
       class="text__area__body"
  >
    <div>
      <textarea class="text__area"></textarea>
<!--      <span>{{ input.label }}</span>-->
    </div>
    <button
      @click="deleteField(index)"
      v-if="index !== 0"
      class="text__area__button"
    >+</button>
  </div>
  <button @click="addNewField">Добавить</button>
</template>

<script>
export default {
  name: "v-textArea",
  emits:["deleteInput","createInput"],
  props: {
    textarea: {
      type: Array,
      default () {
        return []
      }
    }
  },
  methods:{
    deleteField(index){
      this.$emit('deleteInput', index)
    },
    addNewField(){
      this.$emit('createInput')
    },
  }

}
</script>

<style lang="scss" scoped>
.text__area__body{
  position: relative;

  .text__area{
    min-height: 45px;
    min-width: 300px;
    max-width: 300px;
    padding: 12px;
    border-radius: 15px;
    border: none;
    background: #760ec8;
    color: white;

  }

  .text__area__button{
    position: absolute;
    top: 0;
    right: 0;
    height: 20px;
    width: 20px;
    border: none;
    border-radius: 15px;
    rotate: 45deg;
    font-size: 15px;
  }
}


</style>
