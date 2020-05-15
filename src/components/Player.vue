<template>
  <div>
    <h1>{{ title }}</h1>
    <section>
      <h2>Now Playing : {{current.name}}</h2>
      <p>By {{current.artistName}}</p>
      <div class="controls">
        <button @click="prev" :disabled="index <= 0">Prev</button>
        <button v-if="!isPlaying" @click="play">Play</button>
        <button v-else @click="pause">Pause</button>
        <button @click="next" :disabled="index >= songs.length -1">Next</button>
      </div>
    </section>
    <section>
      <h3>Playlist</h3>
      <div v-for="song in songs" :key="song.src">
        <h4>Track : {{song.name}}</h4>
        <p>Artist : {{song.artistName}}</p>
        <button @click="play(song)">Play</button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "Player",
  props: {
    title: String
  },
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      //   All music courtesy of https://freemusicarchive.org/
      songs: [],
      player: new Audio()
    };
  },
  created() {
    this.fetchSongs();
  },
  methods: {
    play(song) {
      if (typeof song.previewURL !== "undefined") {
        this.current = song;
        this.player.src = this.current.previewURL;
      }

      this.isPlaying = true;
      this.player.play();
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    prev() {
      this.index <= 0 ? (this.index = 0) : this.index--;
      this.setCurrentSong();
      this.play(this.current);
    },
    next() {
      this.index >= this.songs.length
        ? (this.index = this.songs.length - 1)
        : this.index++;
      this.setCurrentSong();
      this.play(this.current);
    },
    setCurrentSong() {
      this.current = this.songs[this.index];
      this.player.src = this.current.src;
    },
    fetchSongs() {
      // napster api url : https://developer.napster.com/developer
      fetch("https://api.napster.com/v2.1/tracks/top?apikey={YOUR_API_KEY}")
        .then(res => res.json())
        .then(data => {
          this.songs = data.tracks;
          this.current = this.songs[this.index];
          this.player.src = this.current.previewURL;
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body,
html {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
