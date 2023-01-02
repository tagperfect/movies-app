<template>
  <div></div>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  name: 'NotificationMovies',
  computed: {
    ...mapGetters(['messagePool']),
  },
  watch: {
    messagePool: 'showNotification',
  },
  methods: {
    showNotification({
      variant = 'danger',
      msg = 'default msg',
      title = 'default title',
    }) {
      console.log('click');
      this.$bvToast.toast(msg, {
        title,
        variant,
        solid: true,
      });
    },
  },
};
</script>

<style scoped></style>
