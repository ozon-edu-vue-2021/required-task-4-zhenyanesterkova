<template>
  <div class="form">
    <div class="row">
      {{ formData }}
    </div>

    <h2>Личные данные</h2>

    <div class="row">
      <label for="userSurname" class="form-label col-lg-4"
        >Фамилия<input
          id="userSurname"
          type="text"
          class="form-control"
          v-model="formData.userSurname"
      /></label>
      <label class="form-label col-lg-4" for="userName">
        Имя
        <input
          class="form-control"
          id="userName"
          type="text"
          v-model="formData.userName"
        />
      </label>
      <label for="userPatronymic" class="form-label col-lg-4">
        Отчество
        <input
          type="text"
          id="userPatronymic"
          class="form-control"
          v-model="formData.userPatronymic"
        />
      </label>
    </div>

    <div class="row">
      <label for="userDateOfBirth" class="form-label col-lg-12">
        Дата рождения
        <input
          type="date"
          id="userDateOfBirth"
          class="form-control"
          v-model="formData.userDateOfBirth"
        />
      </label>
    </div>

    <div class="row">
      <label for="userEmail" class="form-label col-lg-12">
        Email
        <input
          type="email"
          id="userEmail"
          class="form-control"
          v-model="formData.userEmail"
        />
      </label>
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
      <label for="userCitizenship" class="form-label col-lg-12">
        Гражданство
        <select
          class="form-select"
          id="userCitizenship"
          v-model="formData.userCitizenship"
          multiple
        >
          <option
            v-for="citizenship in allCitizenships"
            :key="citizenship.id"
            v-bind:value="citizenship.id"
          >
            {{ citizenship.nationality }}
          </option>
        </select>
      </label>
    </div>
    <div class="userPassport">
      <div class="russionPassport" v-if="isHaveRussiaCitizenship">
        <h5>Паспорт гражданства Russia</h5>
        <div class="row">
          <label for="seriesOfRussianPassport" class="form-label col-lg-3"
            >Серия
            <input
              id="seriesOfRussianPassport"
              type="text"
              class="form-control"
              v-model="formData.russianPassport.series"
          /></label>
          <label for="numberOfRussianPassport" class="form-label col-lg-3"
            >Номер<input
              id="numberOfRussianPassport"
              type="text"
              class="form-control"
              v-model="formData.russianPassport.number"
          /></label>
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
        <h5>Иностранные паспорта</h5>
        <div
          v-for="citizenship in formData.userCitizenship"
          :key="citizenship.id"
        >
          <div class="row">
            <label for="userSurnameInLatin" class="form-label col-lg-6"
              >Фамилия на латинице<input
                id="userSurnameInLatin"
                type="text"
                class="form-control"
                v-model="formData.foreignPassports[0].userSurnameInLatin"
            /></label>
            <label class="form-label col-lg-6" for="userName">
              Имя на латинице
              <input
                class="form-control"
                id="userNameInLatin"
                type="text"
                v-model="formData.foreignPassports[0].userNameInLatin"
              />
            </label>
          </div>
          <div class="row">
            <label for="numberOfForeignPassport" class="form-label col-lg-4"
              >Номер паспорта
              <input
                id="numberOfForeignPassport"
                type="text"
                class="form-control"
                v-model="formData.foreignPassports[0].number"
            /></label>
            <label for="countryOfPassport" class="form-label col-lg-3">
              Страна выдачи
              <select
                class="form-select"
                id="countryOfPassport"
                v-model="formData.foreignPassports[0].countryOfIssue"
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
                v-model="formData.foreignPassports[0].type"
              >
                <option
                  v-for="typeOfPassport in allTypesOfPassports"
                  :key="typeOfPassport.id"
                  v-bind:value="typeOfPassport.id"
                >
                  {{ typeOfPassport.type }}
                </option>
              </select>
            </label>
          </div>
        </div>
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
  </div>
</template>

<script>
import citizenships from "@/assets/data/citizenships.json";
import typesOfPassports from "@/assets/data/passport-types.json";

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

        userCitizenship: [],
        russianPassport: {
          series: "",
          number: "",
          dateOfIssue: "",
        },
        foreignPassports: [
          {
            userSurnameInLatin: "",
            userNameInLatin: "",
            number: "",
            countryOfIssue: "",
            type: "",
          },
        ],
        userChangeSurname: "false",
      },
      allCitizenships: citizenships,
      allTypesOfPassports: typesOfPassports,
    };
  },
  computed: {
    isHaveRussiaCitizenship: function () {
      if (
        this.formData.userCitizenship.length > 0 &&
        this.formData.userCitizenship.includes(8604)
      ) {
        return true;
      }
      return false;
    },
    isHaveForeignCitizenship: function () {
      if (this.formData.userCitizenship.length > 1) {
        return true;
      } else if (this.formData.userCitizenship.length === 0) {
        return false;
      } else if (!this.isHaveRussiaCitizenship) {
        return true;
      }
      return false;
    },
  },
  methods: {},
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
.form div {
  margin-top: 20px;
}
</style>
