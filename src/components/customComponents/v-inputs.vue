<template>
  <div class="input-container"
       v-for="field in fields"
       :key="field.id">
    <label class="input-container">
      <input class="input__text"
             :type="field.type"
             :class="{'wrong-input': field.valid === false,'right-input': field.valid === true,'notUsed-input': field.valid === 'notUsed'}"
             :pattern="field.pattern"
             :required="field.required"
             v-model="field.value"/>
      <span class="input__label"
            :class="{'req-label': field.required}"

      >{{ field.label }}</span>
    </label>
  </div>
</template>

<script>
export default {
  name: "vueInputs",
  props: {
    fields: {
      type: Array,
      default () {
        return []
      }
    },
  },
  methods: {
  }
}
</script>

<style lang="scss" scoped>
 .input-container {
   position: relative;
   padding: 15px 0;
   max-width: 400px;

   input {
     border: #5b2d82 solid 2px;
     border-radius: 10px;
     padding: 6px 12px;
     height: 25px;
     width: 265px;
     &:focus-visible{
       outline: #bdbdbd solid;
       border: 3px solid #5b2d82;
     }
   }
   .wrong-input {
     outline: #f77d7d solid;
     border: 2px solid red;

     &:focus-visible{
       outline: #ff0d0d solid;
       border: 4px solid #7c0000;
     }
   }
   .right-input {
     outline: #85f77d solid;
     border: 2px solid #00ff33;

     &:focus-visible{
       outline: #11ff00 solid;
       border: 3px solid #00851b;
     }
   }
   .notUsed-input{
     outline: #9d9d9d solid;
     border: 2px solid #9d9d9d;

     &:focus-visible{
       outline: #898888 solid;
       border: 3px solid #545454;
     }
   }
 }
 .req-label{
   color: red;
 }
 .input__text {

 }
 .input__label{
   position: absolute;
   left: -8%;
   top: -24%;
   padding: 3px 8px 0px 8px;
   font-size: 14px;
   background-color: #ffffff;
   border: #5b2d82 solid 2px;
   border-radius: 10px;
 }
</style>
