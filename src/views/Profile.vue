<template>
  <div>
    <div class="page-title">
      <h3>{{ 'ProfileTitle' | localize }}</h3>
    </div>

    <form class="form" @submit.prevent="submitHandler">
      <div class="input-field">
        <input
            id="description"
            type="text"
            v-model="name"
            :class="{
              invalid: $v.name.$dirty && !$v.name.required
            }"
        >
        <label for="description">{{ 'Name' | localize }}</label>
        <small
          class="helper-text invalid"
          v-if="$v.name.$dirty && !$v.name.required"
        >{{ 'Message_EnterName' | localize }}</small>
      </div>

      <div class="switch">
        <label>
          English
          <input type="checkbox" v-model="isRuLocale">
          <span class="lever"></span>
          Русский
        </label>
      </div>

      <!-- TODO: Добавить возможность смены темы оформления -->

      <button class="btn waves-effect waves-light" type="submit">
        Обновить
        <i class="material-icons right">send</i>
      </button>
    </form>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import { required } from 'vuelidate/lib/validators';

export default {
  metaInfo() {
    return {
      title: this.$title('ProfileTitle'),
    };
  },
  data: () => ({
    name: '',
    isRuLocale: true,
  }),
  validations: {
    name: { required },
  },
  mounted() {
    this.name = this.info.name;
    this.isRuLocale = this.info.locale === 'ru-RU';
    setTimeout(() => {
      // eslint-disable-next-line no-undef
      M.updateTextFields();
    });
  },
  computed: {
    ...mapGetters(['info']),
  },
  methods: {
    ...mapActions(['updateInfo']),

    async submitHandler() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        // eslint-disable-next-line no-useless-return
        return;
      }

      try {
        await this.updateInfo({
          name: this.name,
          locale: this.isRuLocale ? 'ru-RU' : 'en-US',
        });
      // eslint-disable-next-line no-empty
      } catch (e) {
      }
    },
  },
};
</script>

<style scoped>
  .switch {
    margin-bottom: 2rem;
  }
</style>
