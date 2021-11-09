<template>
  <div class="form">
    <h2>Личные данные</h2>

    <div class="row">
      <label for="userSurname" class="form-label col-lg-4"
        >Фамилия<input id="userSurname" type="text" class="form-control"
      /></label>
      <label class="form-label col-lg-4" for="userName">
        Имя <input class="form-control" id="userName" type="text" />
      </label>
      <label for="userPatronymic" class="form-label col-lg-4">
        Отчество
        <input type="text" id="userPatronymic" class="form-control" />
      </label>
    </div>

    <div class="row">
      <label for="userDateOfBirth" class="form-label col-lg-12">
        Дата рождения
        <input type="date" id="userDateOfBirth" class="form-control" />
      </label>
    </div>

    <div class="row">
      <label for="userEmail" class="form-label col-lg-12">
        Email
        <input type="email" id="userEmail" class="form-control" />
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
        />
      </label>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
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
