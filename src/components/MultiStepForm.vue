<template>
  <div class="container" @click.self="close">
    <article>
      <section :class="animation" v-if="activeStep == 0">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="form-box">
          <div class="input-fields">
              <vueInputs :fields="formSteps[activeStep].fields"/>
          </div>
          <div class="checkbox-container">
            <h3>{{ formSteps[activeStep].subtitle }}</h3>
            <vue-gender-checkbox :checkbox="formSteps[activeStep].checkbox"></vue-gender-checkbox>
          </div>
        </div>
      </section>
      <section :class="animation" v-if="activeStep == 1">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="form-box">
          <div class="">
            <vueSelect
              :options="formSteps[activeStep].options"
            />
            <v-multi-select
              :multiOptions="formSteps[activeStep].multiOptions"
            />
          </div>
        </div>
      </section>
      <section :class="animation" v-if="activeStep == 2">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="form-box">
          <vue-textarea
            :textarea="formSteps[activeStep].textarea"
            @delete-input="deleteThisField"
            @create-input="createInput"
            >

          </vue-textarea>
        </div>
      </section>
      <section :class="animation" v-if="activeStep == 3">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="form-box">

        </div>
      </section>
      <section :class="animation" v-if="activeStep == 4">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="form-box">

        </div>
      </section>

      <v-progress-steps
        :activeStep="activeStep"
        :formStep="formSteps"
        @back-step="backStep"
        @check-valid="nextStep"
        @send-it="sendIt"
      >

      </v-progress-steps>
    </article>
  </div>
</template>

<script>
import vueGenderCheckbox from "@/components/customComponents/v-gendercheckbox.vue"
import vueSelect from "@/components/customComponents/select.vue"
import vueInputs from "@/components/customComponents/v-inputs.vue"
import vProgressSteps from "@/components/customComponents/vProgressSteps"
import vMultiSelect from "@/components/customComponents/v-multiselect"
import vueTextarea from "@/components/customComponents/v-textArea"
export default {
  components: {vueSelect, vueGenderCheckbox, vueInputs, vProgressSteps,vMultiSelect,vueTextarea},
  data: () => {
    return {
      activeStep:0,
      animation: 'animate-in',
      formSteps: [
        {
          title: "???????????? ????????????????????",
          fields: [
            {label: "??????????????:*", value: '', valid:'unknown',pattern:/^[a-zA-Z??-??????-????]+$/u,required:true, type: 'text'},
            {label: "??????:*", value: '', valid:'unknown',pattern:/^[a-zA-Z??-??????-????]+$/u,required:true, type:'text'},
            {label: "Email:", value: '', valid:'unknown',pattern:/[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z0-9]+/,required:false, type:'email'},
            {label: "???????? ????????????????:*", value: '', valid:'unknown',pattern:/.+/,required:true, type:'date'},
            {label: "?????????? ????????????????:*", value: '', valid:'unknown',pattern:/^((8|\+7)[- ]?)?(\(?\d{3}\)?[- ]?)?[\d\- ]{7,10}$/,required:true, type:'text'},
          ],

          subtitle: "?????? ??????:",
          checkbox: [
            {label: "??", id: 1, value: "??",},
            {label: "??", id: 2, value: "??",},
          ]
        },
        {
          title: "???????????????????? ?? ??????????????????",
          fields: [],
          options: [
            {value: '??????????????', id:1},
            {value: '????????????-??????????????????????',id:2},
            {value: '???????????????????????? ????????????',id:3},
            {value: '????????????????',id:4},
            {value: '????????????????????',id:5},
            {value: '????????????????',id:6},
          ],
          multiSelector: {name: "???????????????? ????????????????????:"},
          multiOptions: [
            {value: 'Vue',},
            {value: 'React',},
            {value: 'Angular',},
            {value: 'Svelte',},
            {value: 'Ember.js',},
          ],
          h2: "?????? ????????????????????:",
          checkbox: [
            {label: ""},
          ],
        },
        {
          title: "???????? ????????????",
          fields: [],
          textarea:[
            {label: '?????? ???????? ????????????', value: ''}
          ]
        },
        {
          title: "?????????????? GitHub",
          fields: [
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            <button type="button"></button>
          ]
        },
        {
          title: "?????????????????? ???????????? ???????? ?????????????? ???? ??????????????????!",
          fields: [

          ]
        },
      ]
    }
  },
  methods: {
    createInput(){
      this.formSteps[this.activeStep].textarea.push({label: '?????? ???????? ????????????', value: ''})
    },
    deleteThisField(index){
      this.formSteps[this.activeStep].textarea.splice(1,1)
      console.log(index)
      console.log(this.formSteps[this.activeStep].textarea)
    },
    sendIt(){
      console.log('Hi')
    },
    nextStep() {
      this.animation = 'animate-out';
      setTimeout(() => {
        this.animation = 'animate-in';
        this.activeStep += 1;
      }, 600);
    },
    backStep() {
      this.activeStep -= 1;
      this.animation = 'animate-back';
      setTimeout(() => {
        this.animation = '';
      }, 600);
    },
    checkValid() {
      let valid = true;
      const unRequiredFields = this.formSteps[this.activeStep].fields.filter(({ required }) => required === false)
      unRequiredFields.forEach(field => {
        if(field.pattern.test(field.value)){
          field.valid = true;
        }else{
          field.valid = 'notUsed';
          field.value = '';
        }
      });
      const requiredFields = this.formSteps[this.activeStep].fields.filter(({ required }) => required === true)
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
  },

}
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css2?family=Noto+Serif:ital,wght@0,400;0,700;1,400;1,700&display=swap');
  @mixin flexbox() {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .container {
    @include flexbox();
    overflow: hidden;
    width: 100%;
    min-height: 98vh;
    background: radial-gradient(#760ec8, rgba(128, 86, 185, 0.87)) ;
  }

  article{
    @include flexbox();
    flex-direction: column;
    margin: 10px;
    width: calc(100% - 20px);
    max-width: 720px;
    min-height: 480px;
    perspective: 1500px;


    section {
      @include flexbox();
      flex-direction: column;
      width: 450px;
      min-height: 450px;
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 20px rgb(0 0 0 / 20%);

      .form-box {
        flex-grow: 1;
        width: 100%;

        display: flex;
        align-items: center;
        flex-direction: column;
      }

      h2 {
        font-size: 1.6rem;
        color: #5b2d82;
        margin: 0;
        padding: 20px;
      }
      .input-fields {
        @include flexbox();
        flex-direction: column;
        width: 100%;
      }

      .checkbox-container {
        display: flex;
        align-items: center;
        padding: 20px 0;
        min-width: 250px;
      }
    }
  }

  .animate-in {
    transform-origin: left;
    animation: in .7s ease-in-out;
  }
  .animate-out {
    transform-origin: right;
    animation: out .7s ease-in-out;
  }
  .animate-back {
    transform-origin: right;
    animation: back .7s ease-in-out;
  }
  .animate-wrong {
    animation: wrong .7s ease-in-out;
  }

  @keyframes in {
    0% {
      opacity: 0;
      transform: translate(650px) rotateY(70deg);
    }
    100% {
      opacity: 1;
      transform: translate(0px) rotateY(0deg);
    }
  }
  @keyframes out {
    0% {
      opacity: 0;
      transform: translate(0px) rotateY(0deg);
    }
    100% {
      opacity: 1;
      transform: translate(-650px) rotateY(-70deg);
    }
  }
  @keyframes back {
    0% {
      opacity: 0;
      transform: translate(-650px) rotateY(-70deg);
    }
    100% {
      opacity: 1;
    }
  }
  @keyframes wrong {
    0% { transform: translateX(0); }
    20% { transform: translateX(5px); }
    40% { transform: translateX(-35px); }
    60% { transform: translateX(25px); }
    80% { transform: translateX(-15px); }
    100% { transform: translateX(0); }
  }
</style>
