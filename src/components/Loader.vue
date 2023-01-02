<template>
  <div class="loader-wrap" v-if="isShowLoader">
    <b-spinner type="grow" variant="light" label="Loading..."></b-spinner>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  name: 'LoaderVue',
  computed: {
    ...mapGetters(['isShowLoader']),
  },
};
</script>

<style scoped>
.loader-wrap {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}
</style>
