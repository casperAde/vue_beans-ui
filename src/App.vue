<template>
  <router-view/>
</template>

<script lang="ts">
import {provide, ref} from 'vue';
import {router} from './router';

export default {
  name: 'App',

  setup() {
    const width = document.documentElement.clientWidth;

    let asideVisible = ref(width > 500);
    provide('asideVisible', asideVisible);

    router.afterEach(() => {
      if (width <= 500) {
        asideVisible.value = false;
      }
    });
  }
};
</script>

<style>

</style>
