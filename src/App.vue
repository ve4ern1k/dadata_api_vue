<template>
  <v-app :style="{ backgroundImage: 'url(' + require('@/assets/ground.jpg') + ')' }">
    <SearchForm />
  </v-app>
</template>

<script>
import SearchForm from '@/components/SearchForm.vue';

export default {
  name: 'App',

  components: {
    SearchForm,
  },
};
</script>

<style>

</style>
