<template>
    <label class="check"
           :id="checkbox.id"
           @click="check"
    >
      <input class="check__input" type="checkbox" v-model="isCheckM"/>
      <span class="check__box"></span>
      <span class="check__box__label">{{ checkbox[0].label }}</span>
    </label>
    <label class="check"
           :id="checkbox.id"
           @click="check"
    >
      <input class="check__input" type="checkbox" v-model="isCheckF"/>
      <span class="check__box"></span>
      <span class="check__box__label">{{ checkbox[1].label }}</span>
    </label>
</template>

<script>
  export default {
    name: "vueGenderCheckbox",
    data() {
      return {
        isCheckM: false,
        isCheckF: false,
      }
    },
    props:{
      checkbox: {
        type: Array,
        default () {
          return []
        }
      }
    },
    methods: {
      check(){
        if(this.isCheckM === false){
          this.isCheckF = false
        }else {
          this.isCheckM = false
        }
      },
    }
  }
</script>

<style lang="scss" scoped>
.check {
  padding-left: 1.2em;
  width: 25px;
  height: 25px;
  margin: 0px 12px;
  display: flex;
  justify-content: center;
  align-items: center;

  .check__box__label{
    margin-left: 25px;
  }
}
.check__input {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0 0 0 0);
}
.check__box {
  margin-top: 0.2em;
  margin-left: -1.2em;
  position: absolute;
  width: 24px;
  height: 24px;
  background-image: url("@/image/checkbox/checkbox.svg");
  background-repeat: no-repeat;
}

/*Checkbox State */
.check__input:checked + .check__box {
  background-image: url("@/image/checkbox/checkbox_s.svg");
}
.check__input:focus + .check__box {
  background-image: url("@/image/checkbox/checkbox_f.svg");
}
.check__input:focus:checked + .check__box {
  background-image: url("@/image/checkbox/checkbox_f_s.svg");
}
.check__input:disabled + .check__box {
  background-image: url("@/image/checkbox/checkbox_d.svg");
}
.check__input:disabled:checked + .check__box {
  background-image: url("@/image/checkbox/checkbox_d_s.svg");
}

</style>
