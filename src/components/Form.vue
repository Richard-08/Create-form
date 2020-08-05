<template>
  <form class="form" @submit.prevent="onSubmit">
    <div class="inputs-container">
      <PersonalityInputs ref="personalityInputs" />

      <AddressInputs ref="addressInputs" />

      <PassportInputs ref="passportInputs" />
    </div>

    <button type="submit" class="form__submit-btn">Создать клиента</button>

    <div class="success-message" :class="{ show: succsesForm }">
      <h2>Клиент успешно создан!</h2>
    </div>
  </form>
</template>

<script>
import PersonalityInputs from './PersonalityInputs';
import AddressInputs from './AddressInputs';
import PassportInputs from './PassportInputs';

export default {
  data() {
    return {
      succsesForm: false,
    };
  },
  components: {
    PersonalityInputs,
    AddressInputs,
    PassportInputs,
  },
  methods: {
    onSubmit() {
      if (
        this.$refs.personalityInputs.$v.$invalid ||
        this.$refs.addressInputs.$v.$invalid ||
        this.$refs.passportInputs.$v.$invalid
      ) {
        this.$refs.personalityInputs.$v.$touch();
        this.$refs.addressInputs.$v.$touch();
        this.$refs.passportInputs.$v.$touch();
        return;
      }
      console.log('ok');

      this.succsesForm = true;
      setTimeout(() => {
        this.succsesForm = false;
      }, 1000);

      const formData = {
        // personality
        surname: this.$refs.personalityInputs.$data.surname,
        name: this.$refs.personalityInputs.$data.name,
        patronymic: this.$refs.personalityInputs.$data.patronymic,
        date: this.$refs.personalityInputs.$data.date,
        phone: this.$refs.personalityInputs.$data.phone,
        gender: this.$refs.personalityInputs.$data.gender,
        groups: this.$refs.personalityInputs.$data.groups
          .filter((value) => value.active)
          .map((value) => value.title),
        doctor: this.$refs.personalityInputs.$data.doctor,
        notSendSms: this.$refs.personalityInputs.$data.notSendSms,
        //address
        index: this.$refs.addressInputs.$data.index,
        country: this.$refs.addressInputs.$data.country,
        region: this.$refs.addressInputs.$data.region,
        city: this.$refs.addressInputs.$data.city,
        street: this.$refs.addressInputs.$data.street,
        home: this.$refs.addressInputs.$data.home,
        //passport
        document: this.$refs.passportInputs.$data.document,
        series: this.$refs.passportInputs.$data.series,
        number: this.$refs.passportInputs.$data.number,
        issuedBy: this.$refs.passportInputs.$data.issuedBy,
        date: this.$refs.passportInputs.$data.date,
      };
      console.log(formData);
    },
  },
};
</script>

<style lang="scss">
@import '../assets/scss/_stylebase.scss';

.form {
  @include media(768px) {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  &__submit-btn {
    display: block;
    padding: 10px 20px;
    margin: 20px auto;
    background-color: $success-color;
    border: 2px solid $success-color;
    color: #fff;
    border-radius: 5px;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s ease;

    &:hover {
      background-color: #fff;
      color: $success-color;
    }
  }
}

.inputs-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 30px;

  @include media(768px) {
    grid-template-columns: 1fr;
  }
}

.input {
  border-bottom: 2px solid $border-base;
  transition: all 0.3s ease;
  padding: 5px;

  &:focus {
    border-bottom: 2px solid $success-color;
  }
}

.label {
  display: block;

  span {
    color: $danger-color;
  }
}

.error {
  display: block;
  font-size: 12px;
  color: $danger-color;
}

.input.error {
  border-color: $danger-color;
  color: $danger-color;
}

.input.success {
  border-color: $success-color;
  color: $success-color;
}

.success-message {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0);
  background-color: $success-color;
  color: #fff;
  text-align: center;
  padding: 10px;
  border-radius: 5px;
  font-weight: bold;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.success-message.show {
  transform: translate(-50%, -50%) scale(1);
}
</style>
