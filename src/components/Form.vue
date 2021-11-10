<template>
  <div>
    <form
      autocomplete="off"
      class="form"
      novalidate
      @submit.prevent="formSubmit"
    >
      <h2>Личные данные</h2>
      <div class="row">
        <label for="userSurname" class="form-label col-lg-4"
          >Фамилия<input
            id="userSurname"
            type="text"
            class="form-control"
            v-model="formData.userSurname"
            @input="onSurnamelInput"
        /></label>
        <span v-if="errors.onSurnamelInput !== ''" class="error">
          {{ errors.onSurnamelInput }}
        </span>
        <label class="form-label col-lg-4" for="userName">
          Имя
          <input
            class="form-control"
            id="userName"
            type="text"
            v-model="formData.userName"
            @input="onNamelInput"
          />
        </label>
        <span v-if="errors.onNamelInput !== ''" class="error">
          {{ errors.onNamelInput }}
        </span>
        <label for="userPatronymic" class="form-label col-lg-4">
          Отчество
          <input
            type="text"
            id="userPatronymic"
            class="form-control"
            v-model="formData.userPatronymic"
            @input="onPatronymiclInput"
          />
        </label>
        <span v-if="errors.onPatronymiclInput !== ''" class="error">
          {{ errors.onPatronymiclInput }}
        </span>
      </div>

      <div class="row">
        <label for="userDateOfBirth" class="form-label col-lg-12">
          Дата рождения
          <input
            type="date"
            id="userDateOfBirth"
            class="form-control"
            v-model="formData.userDateOfBirth"
            @input="onDateOfBirthlInput"
          />
        </label>
        <span v-if="errors.onDateOfBirthlInput !== ''" class="error">
          {{ errors.onDateOfBirthlInput }}
        </span>
      </div>

      <div class="row">
        <label for="userEmail" class="form-label col-lg-12">
          Email
          <input
            type="email"
            id="userEmail"
            class="form-control"
            v-model="formData.userEmail"
            @input="onEmailInput"
          />
        </label>
        <span v-if="errors.onEmailInput !== ''" class="error">
          {{ errors.onEmailInput }}
        </span>
      </div>

      <div>Пол</div>
      <div class="form-check form-check-inline">
        <label class="form-check-label" for="userGenderMale">
          Мужской<input
            name="usergender"
            class="form-check-input"
            type="radio"
            id="userGenderMale"
            value="male"
            v-model="formData.userGender"
          />
        </label>
      </div>
      <div class="form-check form-check-inline">
        <label class="form-check-label" for="userGenderFemale">
          Женский<input
            name="usergender"
            class="form-check-input"
            type="radio"
            id="userGenderFemale"
            value="female"
            v-model="formData.userGender"
          />
        </label>
      </div>

      <div class="row">
        <div class="citizenshipSelector" v-click-outside="hideDropdown">
          <label for="userCitizenship" class="form-label col-lg-12">
            Гражданство
            <input
              id="userCitizenship"
              @focus="isDropdownOpen = true"
              class="form-control"
              v-model="searchCitizenship"
          /></label>
          <div v-if="isDropdownOpen" class="citizenshipSelectorDropdown">
            <ul v-if="allCitizenships.length" class="list-group">
              <li
                v-for="citizenship in allCitizenships"
                :key="citizenship.id"
                class="list-group-item"
                @click="onCitizenshipsClick(citizenship)"
              >
                {{ citizenship.nationality }}
              </li>
            </ul>
            <div v-else class="empty">Ничего не найдено</div>
          </div>
        </div>
      </div>
      <h6 v-if="isSetCitizenship">
        Паспорт гражданина {{ formData.userCitizenship.nationality }}
      </h6>
      <div class="russionPassport" v-if="isHaveRussiaCitizenship">
        <div class="row">
          <label for="seriesOfRussianPassport" class="form-label col-lg-3"
            >Серия
            <input
              id="seriesOfRussianPassport"
              type="text"
              class="form-control"
              v-model="formData.russianPassport.series"
              @input="onSeriesOfPassport"
          /></label>
          <span v-if="errors.onSeriesOfPassport !== ''" class="error">
            {{ errors.onSeriesOfPassport }}
          </span>
          <label for="numberOfRussianPassport" class="form-label col-lg-3"
            >Номер<input
              id="numberOfRussianPassport"
              type="text"
              class="form-control"
              v-model="formData.russianPassport.number"
              @input="onNumberOfRussianPassport"
          /></label>
          <span v-if="errors.onNumberOfRussianPassport !== ''" class="error">
            {{ errors.onNumberOfRussianPassport }}
          </span>
          <label for="dateOfIssueOfRussianPassport" class="form-label col-lg-6">
            Дата выдачи
            <input
              type="date"
              id="dateOfIssueOfRussianPassport"
              class="form-control"
              v-model="formData.russianPassport.dateOfIssue"
            />
          </label>
        </div>
      </div>
      <div class="foreignPassport" v-if="isHaveForeignCitizenship">
        <div class="row">
          <label for="userSurnameInLatin" class="form-label col-lg-6"
            >Фамилия на латинице<input
              id="userSurnameInLatin"
              type="text"
              class="form-control"
              v-model="formData.foreignPassports.userSurnameInLatin"
              @input="onUserSurnameInLatin"
          /></label>
          <span v-if="errors.onUserSurnameInLatin !== ''" class="error">
            {{ errors.onUserSurnameInLatin }}
          </span>
          <label class="form-label col-lg-6" for="userName">
            Имя на латинице
            <input
              class="form-control"
              id="userNameInLatin"
              type="text"
              v-model="formData.foreignPassports.userNameInLatin"
              @input="onUserNameInLatin"
            />
          </label>
          <span v-if="errors.onUserNameInLatin !== ''" class="error">
            {{ errors.onUserNameInLatin }}
          </span>
        </div>
        <div class="row">
          <label for="numberOfForeignPassport" class="form-label col-lg-4"
            >Номер паспорта
            <input
              id="numberOfForeignPassport"
              type="text"
              class="form-control"
              v-model="formData.foreignPassports.number"
          /></label>
          <label for="countryOfPassport" class="form-label col-lg-3">
            Страна выдачи
            <select
              class="form-select"
              id="countryOfPassport"
              v-model="formData.foreignPassports.countryOfIssue"
            >
              <option
                v-for="citizenship in allCitizenships"
                :key="citizenship.id"
                v-bind:value="citizenship.nationality"
              >
                {{ citizenship.nationality }}
              </option>
            </select>
          </label>
          <label for="typeOfPassport" class="form-label col-lg-5">
            Тип паспорта
            <select
              class="form-select"
              id="typeOfPassport"
              v-model="formData.foreignPassports.type"
            >
              <option
                v-for="typeOfPassport in allTypesOfPassports"
                :key="typeOfPassport.id"
                v-bind:value="typeOfPassport.type"
              >
                {{ typeOfPassport.type }}
              </option>
            </select>
          </label>
        </div>
      </div>
      <div>Меняли ли фамилию</div>
      <div class="form-check form-check-inline">
        <label class="form-check-label" for="notChangeSurname">
          Нет<input
            name="userChangeSurname"
            class="form-check-input"
            type="radio"
            id="notChangeSurname"
            value="false"
            v-model="formData.userChangeSurname"
          />
        </label>
      </div>
      <div class="form-check form-check-inline">
        <label class="form-check-label" for="changedSurname">
          Да<input
            name="userChangeSurname"
            class="form-check-input"
            type="radio"
            id="changedSurname"
            value="true"
            v-model="formData.userChangeSurname"
          />
        </label>
      </div>
      <div class="row" v-if="isUserChangeSurname">
        <label for="previousUserSurname" class="form-label col-lg-6"
          >Предыдущая фамилия<input
            id="previousUserSurname"
            type="text"
            class="form-control"
            v-model="formData.previousUserSurname"
            @input="onPreviousSurnameInput"
        /></label>
        <span v-if="errors.onPreviousSurnameInput !== ''" class="error">
          {{ errors.onPreviousSurnameInput }}
        </span>
        <label class="form-label col-lg-6" for="previousUserName">
          Предыдущее имя
          <input
            class="form-control"
            id="previousUserName"
            type="text"
            v-model="formData.previousUserName"
            @input="onPreviousNameInput"
          />
        </label>
        <span v-if="errors.onPreviousNameInput !== ''" class="error">
          {{ errors.onPreviousNameInput }}
        </span>
      </div>
      <div class="row">
        <button class="btn btn-secondary">Сохранить</button>
      </div>
      <span v-if="messageOfErrorValidationForm === true" class="error">
        В форме есть поля, которые заполнены некорректно
      </span>
    </form>
  </div>
</template>

<script>
import citizenships from "@/assets/data/citizenships.json";
import typesOfPassports from "@/assets/data/passport-types.json";
import clickOutside from "vue-click-outside";
import { throttle, debounce } from "../helper.js";

const EMAIL_REG_EXP = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const RUSSIAN_LETTERS = /^[а-яА-Я]+$/;
const SERIES_OF_PASSPORT = /^[0-9]{4}$/;
const NUMBER_OF_RUSSIAN_PASSPORT = /^[0-9]{6}$/;
const ENGLISH_LETTERS = /^[a-zA-Z]+$/;

export default {
  data() {
    return {
      formData: {
        userSurname: "",
        userName: "",
        userPatronymic: "",
        userDateOfBirth: "",
        userEmail: "",
        userGender: "",

        userCitizenship: {},
        russianPassport: {
          series: "",
          number: "",
          dateOfIssue: "",
        },
        foreignPassports: {
          userSurnameInLatin: "",
          userNameInLatin: "",
          number: "",
          countryOfIssue: "",
          type: "",
        },
        userChangeSurname: "true",
        previousUserName: "",
        previousUserSurname: "",
      },
      errors: {
        onSurnamelInput: "",
        onNamelInput: "",
        onPatronymiclInput: "",
        onEmailInput: "",
        onPreviousSurnameInput: "",
        onPreviousNameInput: "",
        onDateOfBirthlInput: "",
        onSeriesOfPassport: "",
        onNumberOfRussianPassport: "",
        onNumberOfForeignPassport: "",
        onUserSurnameInLatin: "",
        onUserNameInLatin: "",
      },
      allCitizenships: citizenships,
      allTypesOfPassports: typesOfPassports,
      isDropdownOpen: false,
      searchCitizenship: "",
      isSetCitizenship: false,
      throttledSearchCitizenship: null,
      debouncedSearchCitizenship: null,
      isValidationForm: false,
      messageOfErrorValidationForm: false,
    };
  },
  computed: {
    isHaveRussiaCitizenship: function () {
      if (this.formData.userCitizenship.nationality === "Russia") {
        return true;
      }
      return false;
    },
    isHaveForeignCitizenship: function () {
      if (
        this.formData.userCitizenship.nationality !== "Russia" &&
        this.formData.userCitizenship.nationality !== undefined
      ) {
        return true;
      }
      return false;
    },
    isUserChangeSurname: function () {
      if (this.formData.userChangeSurname === "true") {
        return true;
      }
      return false;
    },
  },
  methods: {
    formSubmit() {
      this.isValidationForm = true;
      this.messageOfErrorValidationForm = false;
      this.onDateOfBirthlInput();
      this.onSurnamelInput();
      this.onNamelInput();
      this.onPatronymiclInput();
      this.onEmailInput();
      if (this.formData.userChangeSurname === "true") {
        this.onPreviousNameInput();
        this.onPreviousSurnameInput();
      }

      if (this.formData.userCitizenship.nationality === "Russia") {
        this.onSeriesOfPassport();
        this.onNumberOfRussianPassport();
      } else {
        this.onUserSurnameInLatin();
        this.onUserNameInLatin();
      }

      if (this.isValidationForm) {
        console.log("UPDATE API EVENT", this.formData);
      } else {
        this.messageOfErrorValidationForm = true;
      }
    },
    hideDropdown() {
      this.isDropdownOpen = false;
    },
    onCitizenshipsClick(selectedCitizenship) {
      this.formData.foreignPassports = {};
      this.formData.russianPassport = {};
      this.formData.userCitizenship = selectedCitizenship;
      this.isDropdownOpen = false;
      this.isSetCitizenship = true;
      if (selectedCitizenship.nationality !== "Russia") {
        this.formData.foreignPassports.countryOfIssue =
          this.formData.userCitizenship.nationality;
      }
    },
    getCitizenship(searchWord) {
      console.log("FETCH SKILLS EVENT: GET SKILLS FROM API", searchWord);

      this.allCitizenships = citizenships.filter((citizenship) =>
        citizenship.nationality.includes(searchWord)
      );
    },
    onSurnamelInput() {
      if (!RUSSIAN_LETTERS.test(this.formData.userSurname)) {
        this.errors.onSurnamelInput = "Проверьте правильность ввода фамилии";
        this.isValidationForm = false;
      } else {
        this.errors.onSurnamelInput = "";
      }
    },
    onNamelInput() {
      if (!RUSSIAN_LETTERS.test(this.formData.userName)) {
        this.errors.onNamelInput = "Проверьте правильность ввода имени";
        this.isValidationForm = false;
      } else {
        this.errors.onNamelInput = "";
      }
    },
    onPatronymiclInput() {
      if (!RUSSIAN_LETTERS.test(this.formData.userPatronymic)) {
        this.errors.onPatronymiclInput =
          "Проверьте правильность ввода отчества";
        this.isValidationForm = false;
      } else {
        this.errors.onPatronymiclInput = "";
      }
    },
    onDateOfBirthlInput() {
      this.errors.onDateOfBirthlInput = "";
      const dateOfBirth = new Date(this.formData.userDateOfBirth);
      const now = new Date();
      if (now - dateOfBirth < 0) {
        this.errors.onDateOfBirthlInput =
          "Дата рождения не может быть позднее, чем сегодня";
        this.isValidationForm = false;
      }
    },
    onEmailInput() {
      if (!EMAIL_REG_EXP.test(this.formData.userEmail)) {
        this.errors.onEmailInput = "Проверьте правильность ввода email";
        this.isValidationForm = false;
      } else {
        this.errors.onEmailInput = "";
      }
    },
    onPreviousSurnameInput() {
      if (!RUSSIAN_LETTERS.test(this.formData.previousUserSurname)) {
        this.errors.onPreviousSurnameInput =
          "Проверьте правильность ввода предыдущей фамилии";
        this.isValidationForm = false;
      } else {
        this.errors.onPreviousSurnameInput = "";
      }
    },
    onPreviousNameInput() {
      if (!RUSSIAN_LETTERS.test(this.formData.previousUserName)) {
        this.errors.onPreviousNameInput =
          "Проверьте правильность ввода предыдущего имени";
        this.isValidationForm = false;
      } else {
        this.errors.onPreviousNameInput = "";
      }
    },
    onSeriesOfPassport() {
      if (!SERIES_OF_PASSPORT.test(this.formData.russianPassport.series)) {
        this.errors.onSeriesOfPassport =
          "Проверьте правильность ввода серии паспорта";
        this.isValidationForm = false;
      } else {
        this.errors.onSeriesOfPassport = "";
      }
    },
    onNumberOfRussianPassport() {
      if (
        !NUMBER_OF_RUSSIAN_PASSPORT.test(this.formData.russianPassport.number)
      ) {
        this.errors.onNumberOfRussianPassport =
          "Проверьте правильность ввода номера паспорта";
        this.isValidationForm = false;
      } else {
        this.errors.onNumberOfRussianPassport = "";
      }
    },
    onUserSurnameInLatin() {
      if (
        !ENGLISH_LETTERS.test(this.formData.foreignPassports.userSurnameInLatin)
      ) {
        this.errors.onUserSurnameInLatin =
          "Проверьте правильность ввода фамилии на латинице";
        this.isValidationForm = false;
      } else {
        this.errors.onUserSurnameInLatin = "";
      }
    },
    onUserNameInLatin() {
      if (
        !ENGLISH_LETTERS.test(this.formData.foreignPassports.userNameInLatin)
      ) {
        this.errors.onUserNameInLatin =
          "Проверьте правильность ввода имени на латинице";
        this.isValidationForm = false;
      } else {
        this.errors.onUserNameInLatin = "";
      }
    },
  },
  directives: {
    clickOutside,
  },
  watch: {
    searchCitizenship(newValue) {
      this.debouncedSearchCitizenship(newValue);
    },
  },
  created() {
    this.allCitizenships = citizenships;
    this.throttledSearchCitizenship = throttle(this.getCitizenship, 2000);
    this.debouncedSearchCitizenship = debounce(this.getCitizenship, 2000);
  },
};
</script>

<style scoped>
.form {
  width: 600px;
  margin: 20px auto;
  background-color: rgb(255, 255, 255);
  padding: 30px;
  box-shadow: 0 0 5px 1px rgba(12, 10, 10, 0.1);
}
.form div:not(.citizenshipSelectorDropdown) {
  margin-top: 20px;
}
label[for="userCitizenship"] {
  margin-bottom: 0;
}
h6 {
  margin: 10px 0;
}
.error {
  color: red;
  font-size: 12px;
}
</style>
