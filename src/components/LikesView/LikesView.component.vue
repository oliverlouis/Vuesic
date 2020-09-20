<template>
  <div>
    <section class="likes__modal">
      <svg class="likes__back-icon" @click="closeLikes">
        <use xlink:href="../../assets/img/sprite.svg#icon-chevron-left" />
      </svg>
      <div class="likes__list">
        <div class="likes__list-item" v-for="song in likedSongs" :key="song.title">
          <div class="likes__list-item--details">
            <h4 class="likes__title">{{song.title}}</h4>
            <p class="likes__artist">{{song.artist}}</p>
          </div>
          <div class="likes__list-play">
            <div class="likes__list-icon--circle" @click="likesPlay(song)">
              <img class="likes__list-icon--play" src="../../assets/icons/play-button.svg" />
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  props: ["likedSongs", "showLikes"],

  data() {
    return {
      songToPlay: {}
    };
  },

  methods: {
    closeLikes() {
      this.$emit("close-likes");
    },

    likesPlay(song) {
      this.$emit("likes-play", song);
    }
  }
};
</script>

<style lang="sass" scoped>
@import "./likesView.scss";
</style>