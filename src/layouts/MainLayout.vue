<template>
  <Loader v-if="loading" />
  <div v-else>
    <div class="app-main-layout">
      <Navbar
        @click="isOpen = !isOpen" />
      <Sidebar
        v-model="isOpen" :key="locale" />
      <main class="app-content" :class="{full: !isOpen}">
        <div class="app-page">
          <router-view />
        </div>
      </main>

      <div class="fixed-action-btn">
        <router-link
            class="btn-floating btn-large blue"
            to="/record"
            v-tooltip="'Создать новую запись'">
          <i class="large material-icons">add</i>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/app/Navbar.vue';
import Sidebar from '@/components/app/Sidebar.vue';
import messages from '@/utils/messages';

export default {
  name: 'main-layout',
  data: () => ({
    isOpen: false,
    loading: true,
  }),
  async mounted() {
    if (!Object.keys(this.$store.getters.info).length) {
      await this.$store.dispatch('fetchInfo');
    }
    this.loading = false;
  },
  computed: {
    error() {
      return this.$store.getters.error;
    },
    locale() {
      console.log('this', this);
      return 'ru-RU';
    },
  },
  watch: {
    error(fbError) {
      this.$error(messages[fbError.code] || 'Что-то пошло не так');
    },
  },
  components: {
    Navbar, Sidebar,
  },
};
</script>

<style lang="scss">
  .app-main-layout {
    background-color: #F8F8F8;

    .app-content {
      padding-top: 0;

      .app-page {
        padding-top: 16px;
      }
    }
  }
</style>
