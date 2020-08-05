<template>
  <div class="address-inputs">
    <!-- -----------------------------Index input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="index">Индекс</label>
      <input
        v-model.trim="index"
        type="text"
        name="index"
        class="index-input input"
        :class="{ success: index.length > 0 }"
      />
    </div>

    <!-- -----------------------------Country input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="country">Страна</label>
      <input
        v-model.trim="country"
        type="text"
        name="country"
        class="country-input input"
        :class="{ success: country.length > 0 }"
      />
    </div>

    <!-- -----------------------------Region input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="region">Область</label>
      <input
        v-model.trim="region"
        type="text"
        name="region"
        class="region-input input"
        :class="{ success: region.length > 0 }"
      />
    </div>

    <!-- -----------------------------City input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="city">Город <span>*</span></label>
      <input
        v-model.trim="city"
        type="text"
        name="city"
        class="city-input input"
        :class="{
          error: $v.city.$dirty && !$v.city.required,
          success: $v.city.required,
        }"
      />
      <small v-if="$v.city.$dirty && !$v.city.required" class="error"
        >Обязательное поле для заполнения</small
      >
    </div>

    <!-- -----------------------------Home input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="home">Дом</label>
      <input
        v-model.trim="home"
        type="text"
        name="home"
        class="home-input input"
        :class="{ success: home.length > 0 }"
      />
    </div>

    <!-- -----------------------------Street input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="street">Улица</label>
      <input
        v-model.trim="street"
        type="text"
        name="street"
        class="street-input input"
        :class="{ success: street.length > 0 }"
      />
    </div>
  </div>
</template>

<script>
import { required } from 'vuelidate/lib/validators';

export default {
  data() {
    return {
      index: '',
      country: '',
      region: '',
      city: '',
      street: '',
      home: '',
    };
  },
  validations: {
    city: { required },
  },
};
</script>

<style lang="scss">
.address-inputs > * {
  margin-bottom: 10px;
}
</style>
