<template>
  <div class="passport-inputs">
    <!-- -----------------------------Document select-------------------------------------- -->
    <ul class="dropdown">
      <li
        class="dropdown__title"
        @click="showDocumentsList"
        :class="{ success: $v.documents.validSelect }"
      >
        {{ document }}
        <span>*</span>
      </li>

      <ul class="dropdown__items" :class="{ show: showDocuments }">
        <li
          class="dropdown__item"
          v-for="(document, index) in documents"
          :key="index"
          :class="{ active: document.active }"
          @click="changeDocument(index)"
        >
          {{ document.type }}
        </li>
      </ul>
      <small
        v-if="$v.documents.$dirty && !$v.documents.validSelect"
        class="error"
        >Необходимо выбрать тип документа</small
      >
    </ul>

    <!-- -----------------------------Series input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="series">Серия</label>
      <input
        v-model.trim="series"
        type="text"
        name="series"
        class="series-input input"
        :class="{ success: series.length > 0 }"
      />
    </div>

    <!-- -----------------------------Number input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="number">Номер</label>
      <input
        v-model.trim="number"
        type="text"
        name="number"
        class="number-input input"
        :class="{ success: number.length > 0 }"
      />
    </div>

    <!-- -----------------------------Issued By input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="issuedBy">Кем выдан</label>
      <input
        v-model.trim="issuedBy"
        type="text"
        name="issuedBy"
        class="issuedBy-input input"
        :class="{ success: issuedBy.length > 0 }"
      />
    </div>

    <!-- -----------------------------Date input-------------------------------------- -->
    <div class="input-field">
      <label class="label" for="date">Дата выдачи <span>*</span></label>
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
        >Необходимо заполнить дату выдачи</small
      >
    </div>
  </div>
</template>

<script>
import { required } from 'vuelidate/lib/validators';

export default {
  data() {
    return {
      documents: [
        {
          type: 'Паспорт',
          active: false,
        },
        {
          type: 'Свидетельство о рождении',
          active: false,
        },
        {
          type: 'Вод. удостоверение',
          active: false,
        },
      ],
      showDocuments: false,
      document: 'Тип документа',
      series: '',
      number: '',
      issuedBy: '',
      date: '',
    };
  },
  validations: {
    documents: {
      validSelect: (value) => value.some((item) => item.active),
    },
    date: { required },
  },
  methods: {
    showDocumentsList() {
      this.showDocuments = !this.showDocuments;
    },
    changeDocument(index) {
      this.document = this.documents[index].type;
      this.documents.map((value, i) =>
        i === index ? (value.active = true) : (value.active = false)
      );

      this.showDocuments = false;
    },
  },
};
</script>

<style lang="scss">
.passport-inputs > * {
  margin-bottom: 10px;
}
</style>
