<template>
    <label class="check">
      <input class="check__input" type="checkbox"/>
      <span class="check__box"></span>
      <slot></slot>
    </label>
</template>

<script>
  export default {
    name: "vueCheckbox",
    data() {
      return {
      checkboxCheck: false
      }
    },
  }
</script>

<style scoped>
.check {
padding-left: 1.2em;
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
