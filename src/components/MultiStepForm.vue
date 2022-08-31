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
          </div>
        </div>
      </section>
      <section :class="animation" v-if="activeStep == 2">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="form-box">

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
        @check-valid="checkValid"
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
export default {
  components: {vueSelect, vueGenderCheckbox, vueInputs, vProgressSteps},
  data: () => {
    return {
      activeStep:0,
      animation: 'animate-in',
      formSteps: [
        {
          title: "Личная информация",
          fields: [
            {label: "Фамилия:*", value: '', valid:'unknown',pattern:/^[a-zA-Zа-яёА-ЯЁ]+$/u,required:true, type: 'text'},
            {label: "Имя:*", value: '', valid:'unknown',pattern:/^[a-zA-Zа-яёА-ЯЁ]+$/u,required:true, type:'text'},
            {label: "Email:", value: '', valid:'unknown',pattern:/[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z0-9]+/,required:false, type:'email'},
            {label: "Дата рождения:*", value: '', valid:'unknown',pattern:/.+/,required:true, type:'date'},
            {label: "Номер телефона:*", value: '', valid:'unknown',pattern:/^((8|\+7)[- ]?)?(\(?\d{3}\)?[- ]?)?[\d\- ]{7,10}$/,required:true, type:'text'},
          ],

          subtitle: "Ваш пол:",
          checkbox: [
            {label: "М", id: 1, value: "М",},
            {label: "Ж", id: 2, value: "Ж",},
          ]
        },
        {
          title: "Информация о кандидате",
          fields: [],
          options: [
            {value: 'Среднее', id:1},
            {value: 'Средне-специальное',id:2},
            {value: 'Неоконченное высшее',id:3},
            {value: 'Бакалавр',id:4},
            {value: 'Магистрант',id:5},
            {value: 'Аспирант',id:6},
          ],
          multiSelector: {name: "Знакомые фреймворки:"},
          multiOptions: [
            {value: 'Среднее',},
            {value: 'Средне-специальное',},
            {value: 'Неоконченное высшее',},
            {value: 'Бакалавр',},
            {value: 'Магистрант',},
            {value: 'Аспирант',},
          ],
          h2: "Смс оповещение:",
          checkbox: [
            {label: ""},
          ],
        },
        {
          title: "Опыт работы",
          fields: [
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
          ]
        },
        {
          title: "Профиль GitHub",
          fields: [
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            <button type="button"></button>
          ]
        },
        {
          title: "Проверьте каждое поле которое вы заполнили!",
          fields: [

          ]
        },
      ]
    }
  },
  methods: {
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

        @include flexbox();
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

        .flex__checkbox {
          display: flex;
          flex-grow: 1;
        }
        .h2 {
          font-family: 'Noto Serif', serif;
          font-size: 1.35rem;
          pointer-events: none;
        }

        .checkbox {
          font-family: "Noto Serif", serif;
          font-size: 1.35rem;
          width: 100%;
          display: flex;
          align-items: center;
          justify-content: center;
        }
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
