<template>
  <div class="container">
    <article>
      <header>
        <button v-if="activeStep -1 >= 0" @click="backStep">Вернутся</button>
        <div class="progress">

          <div class="progress-step"
          :class="{'active':index === activeStep}"
          v-for="(step, index) in formSteps"
          :key="'step'+index">
            {{ index + 1 }}
          </div>
        </div>
        <button v-if="activeStep +1 < formSteps.length" @click="checkValid">Продолжить</button>
        <button v-if="activeStep +1 === formSteps.length" @click="checkValid">Отправить</button>
      </header>
      <section :class="animation">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="input-fields">
          <div class="input-container"
          v-for="(field, index) in formSteps[activeStep].fields"
          :key="'field'+ index">
            <input type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" required>
            <label class="input-label">{{ field.label }}</label>
          </div>
        </div>
        <div class="checkbox-fields">
          <div class="checkbox-container">
            <span class="h2">Ваш пол:</span>
            <div class="checkbox"
                 v-for="(checkbox, index) in formSteps[activeStep].checkbox"
                 :key="'checkbox'+ index">
              <input type="checkbox" v-model="checkbox.value" required>
              <label class="checkbox-label">{{ checkbox.label }}</label>
            </div>
          </div>
        </div>
      </section>
    </article>
  </div>
</template>

<script>

export default {
  data: () => {
    return {
      activeStep:0,
      animation: 'animate-in',
      formSteps: [
        {
          title: "Личная информация",
          fields: [
            {label: "Фамилия", value: '', valid:true,pattern:/^[a-zA-Zа-яА-Я]+$/ui,required:true},
            {label: "Имя", value: '', valid:true,pattern:/^[a-zA-Zа-яА-Я]+$/ui,required:true},
            {label: "email", value: '', valid:true,pattern:/^[А-ЯA-Z0-9._%+-]+@[А-ЯA-Z0-9-]+.+.[A-Z]{2,4}$/i},
            {label: "Дата рождения", value: '', valid:true,pattern:/.+/,required:true},
            {label: "Номер телефона", value: '+7', valid:true,pattern:/.+/,required:true},
          ],
          checkbox: [
            {label: "М"},
            {label: "Ж"},
          ],
        },
        {
          title: "Информация о кандидате",
          fields: [
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
            {label: "Some some some some some", value: '', valid:true,pattern:/.+/},
          ]
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
    flex-direction: column-reverse;
    margin: 10px;
    width: calc(100% - 20px);
    max-width: 720px;
    min-height: 480px;
    perspective: 1500px;

    header {
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
    section {
      @include flexbox();
      flex-direction: column;
      width: 450px;
      min-height: 450px;
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 20px rgb(0 0 0 / 20%);

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
      .checkbox-fields {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 100%;
      }
      .checkbox-container {
        display: flex;
        align-items: center;
        padding: 20px 20px 20px 20px;
        width: calc(100% - 40px);
        max-width: 400px;

        .h2 {
          font-family: 'Noto Serif', serif;
          font-size: 1.35rem;
          pointer-events: none;
        }

        .checkbox {
          padding-left: 15px;
          font-family: 'Noto Serif', serif;
          font-size: 1.35rem;
        }
      }

      .input-container {
        position: relative;
        padding: 20px 20px 20px 20px;
        width: calc(100% - 40px);
        max-width: 400px;

        input {
          position: relative;
          width: 100%;
          font-family: 'Noto Serif', serif;
          font-size: 1.25rem;
          outline: none;
          background: transparent;
          box-shadow: none;
          border: none;
          border-bottom: 2px dashed #5b2d82;

          &:valid + .input-label {
            top: 8px;
            left: 16px;
            font-size: .7rem;
            font-weight: normal;
            color: #999;
          }
          &.wrong-input + .input-label {
            color: red;
          }
        }
      }

      .input-label {
        position: absolute;
        left: 22px;
        top: 20px;
        font-family: 'Noto Serif', serif;
        font-size: 1.35rem;
        pointer-events: none;
        transition: .2s ease-in-out;
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
