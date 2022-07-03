<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ currentSong.title }} - <span>{{ currentSong.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button 
          v-for="song in songs" 
          :key="song.src" 
          @click="play(song)"
          :class="(song.src == currentSong.src) ? 'song playing' : 'song'"
        >
          {{song.title}} - {{song.artist}}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      index: 0,
      player: new Audio(),
      isPlaying: false,
      currentSong: {},
      songs: [
        {
          id: 0,
          title: "Bir Hoş Bakar",
          artist: "Ankaralı Namık",
          src: require("./assets/bir-hos-bakar.mp3"),
        },
        {
          id: 1,
          title: "Hüdayda",
          artist: "Ankaralı Namık",
          src: require("./assets/hudayda.mp3"),
        },
        {
          id: 2,
          title: "Cahildim Dünyanın Rengine Kandım",
          artist: "Neşet Ertaş",
          src: require("./assets/cahildim-dunyanin-rengine-kandim.mp3"),
        },        
      ],
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.index = song.id
        this.currentSong = song;

        this.player.src = this.currentSong.src;
      }     

      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next(){
      this.index++
      if(this.index > this.songs.length - 1){
        this.index = 0
      }

      this.currentSong = this.songs[this.index]
      this.play(this.currentSong)

    },
    prev(){
      this.index--
      if(this.index < 0){
        this.index = this.songs.length - 1
      }

      this.currentSong = this.songs[this.index]
      this.play(this.currentSong)
    }
  },
  created() {
    this.currentSong = this.songs[this.index];
    this.player.src = this.currentSong.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  background-color: #1b1b1b;
  color: #ffa31a;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #ffa31a;
  background-color: #1b1b1b;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #808080;
  transition: all 0.4s;
}

.next:hover, .prev:hover {
  color: #ffa31a;
  background-color: #1b1b1b;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover {
  color: #ffa31a;
}

.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #ffa31a, #FF5858);
}
</style>
