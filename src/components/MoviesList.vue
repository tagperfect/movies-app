<template>
  <BContainer>
    <h3 class="list-title">{{ listTitle }}</h3>
    <BRow>
      <template v-if="isExist">
        <BCol cols="3" v-for="(movie, key) in list" :key="key">
          <MovieItem
            :movie="movie"
            @mouseover.native="onMouseOver(movie.Poster)"
            @removeItem="onRemoveItem"
            @showModal="onShowMovieInfo"
          />
        </BCol>
      </template>
      <template v-else>
        <div>Empty List</div>
      </template>
    </BRow>
    <BModal
      body-class="movie-modal-body"
      :id="movieInfoModalID"
      size="xl"
      hide-footer
      hide-header
    >
      <ModalInfoContent :movie="selectedMovie" @closeModal="onCloseModal" />
    </BModal>
  </BContainer>
</template>

<script>
import { BCol, BContainer, BModal, BRow } from 'bootstrap-vue';
import { mapActions, mapGetters } from 'vuex';
import MovieItem from './MovieItem';
import ModalInfoContent from './MovieInfoModalContent';

export default {
  name: 'MoviesList',
  props: {
    list: {
      type: Object,
      default: () => ({}),
    },
  },
  data: () => ({
    movieInfoModalID: 'movie-info',
    selectedMovieID: '',
  }),
  components: { BContainer, BRow, BCol, MovieItem, BModal, ModalInfoContent },
  computed: {
    ...mapGetters('movies', ['isSearch']),
    isExist() {
      return Boolean(Object.keys(this.list).length);
    },
    listTitle() {
      return this.isSearch ? 'Search results' : 'IMDB TOP 250';
    },
    selectedMovie() {
      return this.selectedMovieID ? this.list[this.selectedMovieID] : null;
    },
  },
  methods: {
    ...mapActions('movies', ['removeMovie']),
    ...mapActions(['showNotify']),
    onMouseOver(poster) {
      this.$emit('changePoster', poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(
        // eslint-disable-next-line prettier/prettier
        `Are you sure to remove ${title}?`,
      );
      if (isConfirmed) {
        this.removeMovie(id);
        this.showNotify({
          msg: 'Movie deleted successfully',
          variant: 'success',
          title: 'Success',
        });
      }
    },
    onShowMovieInfo(id) {
      this.selectedMovieID = id;
      this.$bvModal.show(this.movieInfoModalID);
    },
    onCloseModal() {
      this.$bvModal.hide(this.movieInfoModalID);
    },
  },
};
</script>

<style scoped>
.list-title {
  font-size: 50px;
  margin-bottom: 30px;
  color: #fff;
}
</style>
<style>
.movie-modal-body {
  padding: 0 !important;
}
.modal-content {
  border: none !important;
}
</style>
