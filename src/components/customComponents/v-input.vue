<template>
  <div class="input-container"
       v-for="field in fields"
       :key="field.id">
    <label class="input-container">
      <input class="input__text"
             :type="field.type"
             :class="{'wrong-input': !field.valid}"
             :pattern="field.pattern"
             :required="field.required"
             :value="field.value"/>
      <span class="input__label">{{ field.label }}</span>
    </label>
  </div>
</template>

<script>
export default {
  name: "vueInput",
  props: {
    fields: {
      type: Array,
      default () {
        return []
      }
    }
  },
  methods: {
    checkValid() {
      let valid = true;
      const requiredFields = this.fields.filter(({ required }) => required === true)
      requiredFields.forEach(field => {
        if(!field.pattern.test(field.value)) {
          valid = false;
          field.valid = false;
        }
        else {
          field.valid = true;
        }
      });
      if(valid) {
        this.nextStep();
      }
      else {
        this.animation = 'animate-wrong';
        setTimeout(() => {
          this.animation = '';
        }, 600);
      }
    }
  }
}
</script>

<style lang="scss" scoped>
 .input-container {
   position: relative;
   padding: 15px 0;
   max-width: 400px;

   input {
     border-color: #5b2d82;
     border-radius: 10px;
     padding: 5px 10px;
     height: 20px;
     width: 250px;

    &:required + .input__label{
      background-color: #ffcf47;
    }
     .input__invalid{position: absolute;
       background-color: #c70a36;
       color: #ffffff;
     }
     .input__valid{position: absolute;
       background-color: #13a110;
       color: #ffffff;
     }
   }
 }
 .input__text {

 }
 .input__label{
   position: absolute;
   left: -6%;
   top: -15%;
   padding: 3px 8px 0px 8px;
   font-size: 12px;
   background-color: #ffffff;
   border: #5b2d82 solid 2px;
   border-radius: 10px;
   color: #000000;
 }
</style>
