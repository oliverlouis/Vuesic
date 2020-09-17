<template>
  <div id="app">
    <header class="header">
      <div class="header__main">
        <svg class="header__main-icon">
          <use xlink:href="./assets/img/sprite.svg#icon-chevron-left" />
        </svg>
        <svg class="header__main-icon--logo">
          <use xlink:href="./assets/img/sprite.svg#icon-note" />
        </svg>
        <svg class="header__main-icon">
          <use xlink:href="./assets/img/sprite.svg#icon-menu" />
        </svg>
      </div>
      <h4 class="header__now-playing">Now playing...</h4>
    </header>
    <main>
      <section class="player">
        <svg class="player__icon-left">
          <use xlink:href="./assets/img/sprite.svg#icon-cloud" />
        </svg>
        <svg class="player__icon-right">
          <use xlink:href="./assets/img/sprite.svg#icon-heart" />
        </svg>
        <img :src="current.art" alt class="player__album-art" />
        <div class="player__song-info">
          <h2 class="player__song-title">{{current.title}}</h2>
          <span class="player__artist">{{current.artist}}</span>
        </div>

        <div class="player__fullbar">
          <div class="player__progress" :style="{'width': size + '%'}"></div>
        </div>

        <div class="player__controls">
          <div class="player__controls-icon">
            <img src="./assets/icons/repeat.svg" />
          </div>

          <div class="player__controls-icon">
            <img src="./assets/icons/rewind.svg" @click="prev" />
          </div>

          <div class="player__controls-icon--main" v-if="!isPlaying">
            <img class="play-btn" src="./assets/icons/play-button.svg" @click="play" />
          </div>

          <div class="player__controls-icon--main" v-else>
            <img src="./assets/icons/pause.svg" @click="pause" />
          </div>

          <div class="player__controls-icon">
            <img src="./assets/icons/next.svg" alt class="player__controls-icon" @click="next" />
          </div>

          <div class="player__controls-icon">
            <img src="./assets/icons/random.svg" @click="shuffle(songs)" />
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      size: 0,
      songs: [
        {
          title: "You Set My World on Fire",
          artist: "Loving Caliber",
          src: require("./assets/You Set My World on Fire-(Rogan-Gold-Remix)-Loving-Caliber.mp3"),
          art: require("./assets/img/loving-caliber.jpg")
        },
        {
          title: "Break Me Down",
          artist: "Catiso",
          src: require("./assets/Break-Me-Down-Catiso.mp3"),
          art: require("./assets/img/catiso.jpg")
        },
        {
          title: "For the Spectacle",
          artist: "Power Druid",
          src: require("./assets/For-the-Spectacle-Power-Druid.mp3"),
          art: require("./assets/img/power.druid.jpg")
        },
        {
          title: "Flames",
          artist: "STRLIGHT",
          src: require("./assets/Flames-STRLIGHTmp3.mp3"),
          art: require("./assets/img/strlight.jpg")
        },
        {
          title: "Ganja",
          artist: "Ooyy",
          src: require("./assets/Ganja-Ooyy.mp3"),
          art: require("./assets/img/ooyy.jpeg")
        },
        {
          title: "Get it Right Now",
          artist: "Hallman",
          src: require("./assets/Get It Right Now-Hallman.mp3"),
          art: require("./assets/img/hallmann.jpg")
        },
        {
          title: "Pink and Blue",
          artist: "Oominee",
          src: require("./assets/Pink-And-Blue-oomiee.mp3"),
          art: require("./assets/img/oomiee.jpg")
        }
      ],
      player: new Audio()
    };
  },

  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }

      this.player.play();
      this.isPlaying = true;
      this.player.addEventListener("ended", this.next);
      setInterval(this.updateProgressBar, 20);
    },

    pause() {
      this.player.pause();
      this.isPlaying = false;
    },

    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },

    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },

    // AutoplayNext() {
    //   ;
    // },

    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        const temp = array[i];
        array[i] = array[j];
        array[j] = temp;
      }
      this.songs = array;
      this.index = 0;
      this.next();
      this.player.addEventListener("ended", this.next);
    },

    updateProgressBar() {
      if (this.isPlaying) {
        this.size = parseInt(
          (this.player.currentTime / this.player.duration) * 100
        );
      }
    }
  },

  created() {
    this.index = Math.floor(Math.random() * (this.songs.length - 1));
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
};
</script>

<style lang="sass" scoped>
  @import './scss/main';
</style>
