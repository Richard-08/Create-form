<template>
  <div class="personality-inputs">
    <!-- -------------------------Surname input----------------------------- -->
    <div class="input-field">
      <label class="label" for="surname">Фамилия <span>*</span></label>
      <input
        :class="{
          error: $v.surname.$dirty && !$v.surname.required,
          success: $v.surname.required,
        }"
        v-model.trim="surname"
        type="text"
        name="surname"
        class="surname-input input"
      />
      <small v-if="$v.surname.$dirty && !$v.surname.required" class="error"
        >Обязательное поле для заполнения</small
      >
    </div>

    <!-- -----------------------------Name input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="name">Имя <span>*</span></label>
      <input
        v-model.trim="name"
        :class="{
          error: $v.name.$dirty && !$v.name.required,
          success: $v.name.required,
        }"
        type="text"
        name="name"
        class="name-input input"
      />
      <small v-if="$v.name.$dirty && !$v.name.required" class="error"
        >Обязательное поле для заполнения</small
      >
    </div>

    <!-- -----------------------------Patronymic input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="patronymic">Отчество</label>
      <input
        v-model.trim="patronymic"
        type="text"
        name="patronymic"
        class="patronymic-input input"
        :class="{ success: patronymic.length > 0 }"
      />
    </div>

    <!-- -----------------------------Date input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="date">Дата рождения <span>*</span></label>
      <input
        v-model.trim="date"
        :class="{
          error: $v.date.$dirty && !$v.date.required,
          success: $v.date.required,
        }"
        type="date"
        name="date"
        class="date-input input"
      />
      <small v-if="$v.date.$dirty && !$v.date.required" class="error"
        >Необходимо заполнить дату в формате ДД.ММ.ГГГГ.</small
      >
    </div>

    <!-- -----------------------------Phone input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="phone">Номер телефона <span>*</span></label>
      <input
        v-model.trim="phone"
        :class="{
          error: $v.phone.$dirty && !$v.phone.validNumber,
          success: $v.phone.validNumber,
        }"
        type="text"
        name="phone"
        class="phone-input input"
      />
      <small v-if="$v.phone.$dirty && !$v.phone.validNumber" class="error"
        >Номер телефона должен начинаться с 7 и содержать 11 цифр</small
      >
    </div>

    <!-- -----------------------------Gender input -------------------------------------- -->
    <div class="input-field">
      <label class="label" for="gender">Пол</label>
      <input
        v-model.trim="gender"
        type="text"
        name="gender"
        class="gender-input input"
        :class="{ success: gender.length > 0 }"
      />
    </div>

    <!-- -----------------------------Client gtoup select-------------------------------------- -->
    <ul class="dropdown">
      <li
        class="dropdown__title"
        @click="showGroupItems"
        :class="{
          error: $v.phone.$dirty && !$v.groups.validSelect,
          success: $v.groups.validSelect,
        }"
      >
        Группа клиентов <span>*</span>
      </li>

      <ul class="dropdown__items" :class="{ show: showGroup }">
        <li
          class="dropdown__item"
          v-for="(group, index) in groups"
          :key="index"
          :class="{ active: group.active }"
          @click="changeGroup(index)"
        >
          {{ group.title }}
        </li>
      </ul>

      <small v-if="$v.groups.$dirty && !$v.groups.validSelect" class="error"
        >Необходимо выбрать одну или несколько групп</small
      >
    </ul>

    <!-- -----------------------------Attending doctor select-------------------------------------- -->
    <ul class="dropdown">
      <li
        class="dropdown__title"
        @click="showDoctorsList"
        :class="{ success: selectDoctor }"
      >
        {{ doctor }}
      </li>

      <ul class="dropdown__items" :class="{ show: showDoctors }">
        <li
          class="dropdown__item"
          v-for="(doctor, index) in doctors"
          :key="index"
          :class="{ active: doctor.active }"
          @click="changeDoctor(index)"
        >
          {{ doctor.title }}
        </li>
      </ul>
    </ul>

    <!-- -----------------------------Checkbox input-------------------------------------- -->
    <div class="input-field">
      <input
        v-model="notSendSms"
        type="checkbox"
        name="sms"
        class="sms-input input"
      />
      <label for="sms">Не отправлять СМС</label>
    </div>
  </div>
</template>

<script>
import { required, email, numeric } from 'vuelidate/lib/validators';

export default {
  data() {
    return {
      surname: '',
      name: '',
      patronymic: '',
      date: '',
      phone: '',
      gender: '',
      notSendSms: false,
      groups: [
        {
          title: 'VIP',
          active: false,
        },
        {
          title: 'Проблемные',
          active: false,
        },
        {
          title: 'ОМС',
          active: false,
        },
      ],
      showGroup: false,
      showDoctors: false,
      doctor: 'Лечащий врач',
      doctors: [
        {
          title: 'Иванов',
          active: false,
        },
        {
          title: 'Захаров',
          active: false,
        },
        {
          title: 'Чернышева',
          active: false,
        },
      ],
    };
  },
  validations: {
    surname: { required },
    name: { required },
    date: { required },
    phone: {
      required,
      numeric,
      validNumber: (value) => /^((\+7|7)+([0-9]){10})$/gm.test(value),
    },
    groups: {
      validSelect: (value) => value.some((item) => item.active),
    },
  },
  computed: {
    selectDoctor() {
      return this.doctors.some((value) => value.active);
    },
  },
  methods: {
    showGroupItems() {
      this.showGroup = !this.showGroup;
    },
    changeGroup(index) {
      this.groups[index].active = !this.groups[index].active;
    },
    showDoctorsList() {
      this.showDoctors = !this.showDoctors;
    },
    changeDoctor(index) {
      this.doctor = this.doctors[index].title;
      if (this.doctors[index].active) {
        this.doctors[index].active = false;
        this.doctor = 'Лечащий врач';
      } else {
        this.doctors.map((value, i) =>
          i === index ? (value.active = true) : (value.active = false)
        );
      }

      this.showDoctors = false;
    },
  },
};
</script>

<style lang="scss">
@import '../assets/scss/_stylebase.scss';

.personality-inputs > * {
  margin-bottom: 10px;
}

.dropdown {
  position: relative;
  width: 200px;
  text-align: center;

  &__title {
    padding: 5px;
    cursor: pointer;
    border: 2px solid $border-base;
    border-radius: 5px;
    user-select: none;

    span {
      color: $danger-color;
    }
  }

  &__title.error {
    border-color: $danger-color;
  }

  &__title.success {
    border-color: $success-color;
  }

  &__items {
    position: absolute;
    left: 0;
    top: 110%;
    width: 100%;
    background-color: $border-base;
    border-radius: 5px;
    transform: translateY(-50%) scaleY(0);
    transition: all 0.3s ease;
    z-index: 10;
  }

  &__items.show {
    transform: translateY(0) scaleY(1);
  }

  &__item {
    padding: 5px 0;
    cursor: pointer;
    user-select: none;
    transition: all 0.2s ease;

    &:hover {
      background-color: $success-color;
    }
  }

  &__item.active {
    background-color: $success-color;
  }
}
</style>
