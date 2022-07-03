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
        {
          id: 3,
          title: "short song",
          artist: "TEST",
          src: require("./assets/short-song.mp3"),
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
    // this.player.play()
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


</style>
