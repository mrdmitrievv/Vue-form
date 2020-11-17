<template>
  <form @submit.prevent="registerUser" class="form" novalidate>
    <div class="container">
      <h1 class="main_header">Регистрация пользователя</h1>

      <div v-show="step === 1" class="step">
        <div>
          <label for="surname">Фамилия*</label>
          <input
            @blur="$v.formReg.surname.$touch()"
            :class="{ invalid: $v.formReg.surname.$error }"
            v-model="formReg.surname"
            type="text"
            id="surname"
          />

          <div
            v-if="!$v.formReg.surname.required && $v.formReg.surname.$dirty"
            class="invalid-feedback"
          >
            {{ errorMessage }}
          </div>

          <div v-if="!$v.formReg.surname.alpha" class="invalid-feedback">
            {{ errorOnlyLettersMessage }}
          </div>
        </div>

        <div>
          <label for="name">Имя*</label>
          <input
            @blur="$v.formReg.name.$touch()"
            :class="{ invalid: $v.formReg.name.$error }"
            v-model="formReg.name"
            type="text"
            id="name"
          />
          <div
            v-if="!$v.formReg.name.required && $v.formReg.name.$dirty"
            class="invalid-feedback"
          >
            {{ errorMessage }}
          </div>
          <div v-if="!$v.formReg.name.alpha" class="invalid-feedback">
            {{ errorOnlyLettersMessage }}
          </div>
        </div>

        <div>
          <label for="fullname">Отчество</label>
          <input v-model="formReg.fullname" type="text" id="fullname" />
        </div>

        <div>
          <label for="birthday">Дата рождения*</label>
          <input
            @blur="$v.formReg.birthday.$touch()"
            :class="{ invalid: $v.formReg.birthday.$error }"
            v-model="formReg.birthday"
            type="text"
            id="birthday"
          />
          <div
            v-if="!$v.formReg.birthday.required && $v.formReg.birthday.$dirty"
            class="invalid-feedback"
          >
            {{ errorMessage }}
          </div>
          
          <div v-if="!$v.formReg.birthday.date" class="invalid-feedback">
            {{ errorDateMessage }}
          </div>

        </div>

        <div>
          <label for="telephone">Номер телефона*</label>
          <input
            @blur="$v.formReg.telephone.$touch()"
            :class="{ invalid: $v.formReg.telephone.$error }"
            v-model="formReg.telephone"
            type="text"
            id="telephone"
          />
          <div
            v-if="!$v.formReg.telephone.required && $v.formReg.telephone.$dirty"
            class="invalid-feedback"
          >
            {{ errorMessage }}
          </div>

          <div
            v-if="!$v.formReg.telephone.onlyNumbers"
            class="invalid-feedback"
          >
            {{ errorOnlyNumbersMessage }}
          </div>

          <div
            v-if="!$v.formReg.telephone.mobileNumber"
            class="invalid-feedback"
          >
            {{errorTelephoneMessage}}
          </div>

          <div v-if="!$v.formReg.telephone.minLength" class="invalid-feedback">
            {{ errorLengthMessage }}
          </div>

          <div v-if="!$v.formReg.telephone.maxLength" class="invalid-feedback">
            {{errorLengthMessage}}
          </div>
        </div>

        <div>
          <label>Пол</label>
          <div class="male_choice">
            <input class="radio" type="radio" name="male" checked />
            Мужской
            <input class="radio" type="radio" name="male" />
            Женский
          </div>
        </div>

        <div>
          <label>Группа клиентов</label>
          <select multiple size="3">
            <option>VIP</option>
            <option>Проблемные</option>
            <option>ОМС</option>
          </select>
        </div>

        <div>
          <label>Лечащий врач</label>
          <select>
            <option>Иванов</option>
            <option>Захаров</option>
            <option>Чернышева</option>
          </select>
        </div>

        <div class="check_sms">
          <label>Не отправлять смс</label>
          <input type="checkbox" />
        </div>

        <button
          @click="nextStep"
          :disabled="disabledBtn1"
          :class="{
            disabledBtn:
              $v.formReg.surname.$invalid ||
              $v.formReg.name.$invalid ||
              $v.formReg.birthday.$invalid ||
              $v.formReg.telephone.$invalid,
          }"
          class="btn_next"
          type="button"
        >
          Далее
        </button>
      </div>

      <transition name="slide-fade">
        <div v-show="step === 2" class="step">
          <div>
            <label for="index">Индекс</label>
            <input v-model="formReg.index" type="text" id="index" />
          </div>

          <div>
            <label for="country">Страна</label>
            <input v-model="formReg.country" type="text" id="country" />
          </div>

          <div>
            <label for="region">Область</label>
            <input v-model="formReg.region" type="text" id="region" />
          </div>

          <div>
            <label for="city">Город*</label>
            <input
              @blur="$v.formReg.city.$touch()"
              :class="{ invalid: $v.formReg.city.$error }"
              v-model="formReg.city"
              type="text"
              id="city"
            />

            <div
              v-if="!$v.formReg.city.required && $v.formReg.city.$dirty"
              class="invalid-feedback"
            >
              {{ errorMessage }}
            </div>

            <div v-if="!$v.formReg.city.alpha" class="invalid-feedback">
              {{ errorOnlyLettersMessage }}
            </div>
          </div>

          <div>
            <label for="street">Улица</label>
            <input v-model="formReg.street" type="text" id="street" />
          </div>

          <div>
            <label for="house">Дом</label>
            <input v-model="formReg.house" type="text" id="house" />
          </div>

          <div class="buttons">
            <button
              @click="backStep"
              class="btn_next btn_previous-step"
              type="button"
            >
              Назад
            </button>
            <button
              @click="nextStep"
              :disabled="disabledBtn2"
              :class="{ disabledBtn: $v.formReg.city.$invalid }"
              class="btn_next btn_next-step"
              type="button"
            >
              Далее
            </button>
          </div>
        </div>
      </transition>

      <transition name="slide-fade">
        <div v-show="step === 3" class="step">
          <div>
            <label>Тип документа*</label>
            <select>
              <option>Паспорт</option>
              <option>Свидетельство о рождении</option>
              <option>Водительское удостоверение</option>
            </select>
          </div>

          <div>
            <label for="seria">Серия</label>
            <input v-model="formReg.seria" type="text" id="seria" />
          </div>

          <div>
            <label for="number">Номер</label>
            <input v-model="formReg.number" type="text" id="number" />
          </div>

          <div>
            <label for="organ">Кем выдан</label>
            <input v-model="formReg.organ" type="text" id="organ" />
          </div>

          <div>
            <label for="date">Дата выдачи*</label>
            <input
              @blur="$v.formReg.date.$touch()"
              :class="{ invalid: $v.formReg.date.$error }"
              v-model="formReg.date"
              type="text"
              id="date"
            />

            <div
              v-if="!$v.formReg.date.required && $v.formReg.date.$dirty"
              class="invalid-feedback"
            >
              {{ errorMessage }}
            </div>

            <div v-if="!$v.formReg.date.date" class="invalid-feedback">
              {{ errorDateMessage }}
            </div>

          </div>

          <div class="buttons">
            <button
              @click="backStep"
              class="btn_next btn_previous-step"
              type="button"
            >
              Назад
            </button>
            <button
              :disabled="disabledRegister"
              :class="{ disabledBtn: $v.formReg.date.$invalid }"
              class="btn_next btn_next-step"
              type="submit"
            >
              Зарегистрироваться
            </button>
          </div>
        </div>
      </transition>
    </div>
  </form>
</template>

<script>
import {
  required,
  helpers,
  maxLength,
  minLength,
} from "vuelidate/lib/validators";
const alpha = helpers.regex("alpha", /^[a-zA-Zа-яёА-ЯЁ]*$/);
const date = helpers.regex(
  "date",
  /(0[1-9]|[12][0-9]|3[01])[- /.](0[1-9]|1[012])[- /.](19|20)\d\d/
);
const onlyNumbers = helpers.regex("onlyNumbers", /^[0-9]*$/);
const mobileNumber = helpers.regex("mobileNumber", /^[8]/);

export default {
  data() {
    return {
      step: 1,
      errorMessage: "Поле обязательно для заполнения",
      errorOnlyNumbersMessage:  'Нельзя использовать буквы и другие символы, кроме "."',
      errorOnlyLettersMessage: "Нельзя использовать цифры и другие символы",
      errorLengthMessage: "Номер телефона должен состоять из 11 цифр",
      errorDateMessage: "Дата введена некорректно, формат даты 31.12.2018",
      errorTelephoneMessage: "Номер должен начинаться с 8",
      formReg: {
        surname: "",
        name: "",
        fullname: "",
        birthday: "",
        telephone: "",
        index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        house: "",
        seria: "",
        number: "",
        organ: "",
        date: "",
      },
    };
  },
  computed: {
    disabledBtn1() {
      return (
        this.$v.formReg.surname.$invalid ||
        this.$v.formReg.name.$invalid ||
        this.$v.formReg.birthday.$invalid ||
        this.$v.formReg.telephone.$invalid
      );
    },
    disabledBtn2() {
      return this.$v.formReg.city.$invalid;
    },
    disabledRegister() {
      return this.$v.formReg.date.$invalid;
    },
  },
  methods: {
    nextStep() {
      if (this.step < 3) this.step++;
    },
    backStep() {
      if (this.step > 1) this.step--;
    },
    registerUser() {
      console.log("Регистрация прошла успешно!");
      console.log(this.formReg.name);

      this.step = 1;
      for (let input in this.formReg) {
        this.formReg[input] = "";
      }

      this.$v.$reset();
    },
  },
  validations: {
    formReg: {
      surname: {
        required,
        alpha,
      },
      name: {
        required,
        alpha,
      },
      birthday: {
        required,
        date,
      },
      telephone: {
        required,
        onlyNumbers,
        mobileNumber,
        maxLength: maxLength(11),
        minLength: minLength(11),
      },
      city: {
        required,
        alpha,
      },
      date: {
        required,
        date,
      },
    },
  },
};
</script>

<style>
.slide-fade-enter-active {
  transition: all 0.3s ease;
}

.slide-fade-enter {
  transform: translateX(10px);
  opacity: 0;
}
</style>
