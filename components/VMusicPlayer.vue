<template>
  <div class="container">
    <h2>My music</h2>
    <button
      class="btn btn--show-playlist"
      type="button"
      @click="showPlaylist"
    >
      <span v-if="!isPlayListShown"> Show Playlist &#127926; </span>
      <span v-else>Show Player &#128189;</span>
    </button>

    <div
      class="player"
      :class="{ 'player--playlist': isPlayListShown }"
    >
      <div class="player__front flip flip--front">
        <!-- btn add to favorite -->
        <button
          type="button"
          class="btn btn--favorite"
          :class="{ active: current.favorited }"
          @click="favorite"
        >
          &#129505;
        </button>
        <div
          class="player__img-wrap"
          :style="{ backgroundImage: `url(${current.imgUrl})` }"
        ></div>
        <div class="player__controls controls">
          <button
            class="btn btn--arrow"
            type="button"
            @click="prev"
          >
            &#128072;
          </button>
          <ButtonPlay
            :size="64"
            class="btn btn--play"
            :isPlaying="isPlaying"
            type="button"
            @click="play"
          />
          <button
            class="btn btn--arrow"
            type="button"
            @click="next"
          >
            &#128073;
          </button>
        </div>
        <h3 class="player__song-title song-title">
          <span class="song-title__artist"> {{ current.artist }}</span>
          <span class="song-title__name">
            {{ current.title }}
          </span>
        </h3>
        <!-- progress bar -->
        <div class="progress">
          <span class="progress__time">{{ currentTime }}</span>
          <span class="progress__duration">{{ duration }}</span>
          <div
            class="progress__bar"
            @click="clickProgress"
            ref="progress"
          >
            <div
              class="progress__current"
              :style="{ width: barWidth }"
            ></div>
          </div>
        </div>
      </div>
      <div class="flip flip--back">
        <div class="playlist">
          <h3>The Playlist</h3>
          <div class="playlist__list">
            <button
              type="button"
              v-for="song in songs"
              :key="song.src"
              @click="play(song)"
              class="playlist__song"
              :class="song.src === current.src ? 'playlist__song--playing' : ''"
            >
              {{ song.title }} - {{ song.artist }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VMusicPlayer",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      barWidth: null,
      duration: null,
      currentTime: null,
      player: null,
      isPlayListShown: false,
      songs: [
        {
          title: "Song 1 very long name",
          artist: "Artem Komarov",
          src: "mp3/grabby.mp3",
          favorited: false,
          imgUrl: "1.png",
        },
        {
          title: "Song 2",
          artist: "Ivan",
          src: "mp3/leap.mp3",
          favorited: true,
          imgUrl: "2.png",
        },
        {
          title: "Sandy beach",
          artist: "White noise",
          src: "mp3/sandy-beach.mp3",
          favorited: false,
          imgUrl: "3.png",
        },
      ],
    };
  },
  methods: {
    play(song) {
      if (typeof song.src !== "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      if (this.player.paused) {
        this.player.play();
        this.isPlaying = true;
      } else {
        this.player.pause();
        this.isPlaying = false;
      }
      // this.isPlaying = true;
      // return this.player.play();
    },
    pause() {
      this.isPlaying = false;
      return this.player.pause();
    },
    prev() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    generateTime() {
      let width = (100 / this.player.duration) * this.player.currentTime;
      this.barWidth = width + "%";
      this.circleLeft = width + "%";
      let durmin = Math.floor(this.player.duration / 60);
      let dursec = Math.floor(this.player.duration - durmin * 60);
      let curmin = Math.floor(this.player.currentTime / 60);
      let cursec = Math.floor(this.player.currentTime - curmin * 60);
      if (durmin < 10) {
        durmin = "0" + durmin;
      }
      if (dursec < 10) {
        dursec = "0" + dursec;
      }
      if (curmin < 10) {
        curmin = "0" + curmin;
      }
      if (cursec < 10) {
        cursec = "0" + cursec;
      }
      this.duration = durmin + ":" + dursec;
      this.currentTime = curmin + ":" + cursec;
    },
    updateBar(x) {
      let progress = this.$refs.progress;
      let maxduration = this.player.duration;
      let position = x - progress.offsetLeft;
      let percentage = (100 * position) / progress.offsetWidth;
      if (percentage > 100) {
        percentage = 100;
      }
      if (percentage < 0) {
        percentage = 0;
      }
      this.barWidth = percentage + "%";
      this.circleLeft = percentage + "%";
      this.player.currentTime = (maxduration * percentage) / 100;
      this.isPlaying = true;
      this.player.play();
    },
    clickProgress(e) {
      this.player.pause();
      this.updateBar(e.pageX);
    },
    favorite() {
      this.current.favorited = !this.current.favorited;
    },
    showPlaylist() {
      this.isPlayListShown = !this.isPlayListShown;
    },
  },
  mounted() {
    let vm = this;
    this.current = this.songs[0];
    this.player = new Audio();
    this.player.src = this.current.src;
    this.player.ontimeupdate = function () {
      vm.generateTime();
    };
    this.player.onloadedmetadata = function () {
      vm.generateTime();
    };
    this.player.onended = function () {
      vm.next();
    };
  },
};
</script>

<style lang="scss" scoped>
h2 {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 1rem;
}
.player {
  margin: 0 auto;
  color: #03045e;
  background: #caf0f8;
  font-size: 16px;
  min-width: 320px;
  height: 535px;
  max-width: 375px;
  box-shadow: 0px 15px 35px -5px rgba(50, 88, 130, 0.32);
  border-radius: 15px;
  padding: 30px;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.5s ease 0s;
}

.player__front {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.player__img-wrap {
  margin: 0 auto;
  position: relative;
  border-radius: 15px;
  width: 100%;
  height: 190px;
  background-position: center;
  background-size: cover;
  &::before {
    border-radius: inherit;
    content: "";
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: 1;
    box-shadow: 0px 10px 40px 0px rgb(76 70 124 / 50%);
  }
  &::after {
    content: "";
    background: linear-gradient(45deg, rgb(97, 196, 182), rgb(0, 255, 157));
    border-radius: inherit;
    left: 50%;
    filter: blur(15px);
    opacity: 0.9;
    transform: translateX(-50%);
    width: 75%;
    height: 100%;
    position: absolute;
    z-index: -1;
    bottom: -10px;
  }
}
.player--playlist {
  transform: rotateY(180deg);
  background-color: inherit;
  .flip--front {
    z-index: -1;
    pointer-events: none;
    backface-visibility: hidden;
  }
  .flip--back {
    z-index: 1;
    border-radius: inherit;
    background: #caf0f8;
  }
}
.flip {
  padding: 30px;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
}
.flip--front {
  backface-visibility: hidden;
}
.flip--back {
  transform: rotateY(180deg); /* Вращаем по оси Y */
  backface-visibility: hidden; /* Скрываем обратную поверхность */
}
.player__song-title {
  margin-bottom: 0.5em;
}
.song-title {
  color: #03045e;
  width: 75%;
  position: relative;
  // display: flex;
  align-items: center;
  justify-content: space-between;
  font-weight: 600;
}
.song-title__name {
  color: #0077b6;
  font-weight: 400;
  font-style: italic;
  display: block;
  font-size: 0.85em;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
.song-title__artist {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
.player__controls {
  margin-bottom: 1em;
}
.controls {
  display: flex;
  align-items: center;
  justify-content: center;
}
.btn {
  color: #333;
  background: transparent;
  display: flex;
  align-items: center;
  justify-content: center;
}
.btn--show-playlist {
  font-weight: 600;
  width: auto;
  display: block;
  padding: 0.5rem 1rem;
  font-size: 1em;
  margin: 0 auto 0.5rem;
}
.btn--arrow {
  width: 46px;
  height: 46px;
  font-size: 2em;
}
.btn--favorite {
  position: absolute;
  z-index: 2;
  top: 0.5em;
  right: 0.5em;
  font-size: 2em;
  text-shadow: 0 2px 6px rgba(136, 10, 10, 0.5);
  transition: filter $transition, text-shadow $transition;
  filter: grayscale(1);
  &.active {
    text-shadow: 1px 2px 6px rgb(81 34 233 / 75%);
    filter: grayscale(0);
    // animation: name duration timing-function delay iteration-count direction fill-mode;
    animation: popHeart 0.5s ease-in-out 0s 1 both;
  }
}
.btn--play {
  position: relative;
  z-index: 2;
  width: 64px;
  height: 64px;
  border-radius: 50%;
  border: 0;
  color: #023e8a;

  svg {
    position: absolute;
    width: 100%;
    height: 100%;
  }
}
.progress {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
}
.progress__bar {
  height: 6px;
  width: 100%;
  cursor: pointer;
  background-color: #90e0ef;
  display: inline-block;
  border-radius: 10px;
}
.progress__current {
  height: inherit;
  width: 0%;
  background-color: #0077b6;
  border-radius: 10px;
}
h3 {
  color: #03045e;
  margin-bottom: 0.5em;
}
.playlist__song {
  display: block;
  background: transparent;
  color: #0077b6;
  &:not(:last-child) {
    margin-bottom: 0.5em;
  }
}
.playlist__song--playing {
  color: #03045e;
}
.playlist__list {
  max-height: 370px;
  overflow-y: auto;
}
@keyframes popHeart {
  0% {
    transform: scale(1);
  }
  25% {
    transform: scale(1.2);
    text-shadow: 2px 2px 10px rgb(81 34 233 / 75%);
  }
  50% {
    transform: scale(0.9);
  }
  90% {
    transform: scale(1.05);
    text-shadow: 1px 2px 8px rgb(81 34 233 / 75%);
  }
}
</style>
