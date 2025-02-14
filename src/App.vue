<script>
import { onMounted, onUnmounted, ref } from 'vue';
import Cursor from './components/Cursor.vue';

export default {
  components: {
    Cursor
  }
};
</script>


<template>
  <router-view />
  <Cursor></Cursor>
</template>

<style lang="scss">
  @import './scss/style.scss';
</style>