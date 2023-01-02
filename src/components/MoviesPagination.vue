<template>
  <div class="movies-pagination d-flex justify-content-center">
    <BPagination
      v-model="currentPageModel"
      :per-page="perPage"
      :total-rows="total"
      prev-text="Prev"
      next-text="Next"
    />
  </div>
</template>

<script>
import { BPagination } from 'bootstrap-vue';

export default {
  name: 'MoviesPagination',
  components: { BPagination },
  props: {
    currentPage: {
      type: Number,
      default: 1,
    },
    total: {
      type: Number,
      default: 100,
    },
    perPage: {
      type: Number,
      default: 12,
    },
  },
  computed: {
    currentPageModel: {
      get() {
        return this.currentPage;
      },
      set(value) {
        console.log(value);
        this.$emit('pageChanged', value);
      },
    },
  },
};
</script>

<style scoped>
.movies-pagination {
  margin-top: auto;
}
.movies-pagination >>> .pagination .page-item .page-link {
  background-color: transparent;
  font-size: 12px;
  color: #fff;
  box-shadow: none;
}
.movies-pagination >>> .pagination .page-item.active .page-link {
  border-color: #fff;
  background-color: #fff;
  color: #000;
}
.movies-pagination >>> .pagination .page-item.disabled .page-link {
  color: lightgray;
}
</style>
