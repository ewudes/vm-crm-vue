<template>
  <div id="app">
    <component :is="layout">
      <router-view/>
    </component>
  </div>
</template>

<script>
import EmptyLayout from '@/layouts/EmptyLayout.vue';
import MainLayout from '@/layouts/MainLayout.vue';

export default {
  computed: {
    layout() {
      // eslint-disable-next-line prefer-template
      return (this.$route.meta.layout || 'main') + '-layout';
    },
  },
  components: {
    EmptyLayout, MainLayout,
  },
};
</script>

<style lang="scss">
@import '~materialize-css/dist/css/materialize.min.css';
@import 'assets/index.css';
</style>
