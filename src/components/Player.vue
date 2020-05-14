<template>
  <div>
    <h1>{{ title }}</h1>
    <section>
      <h2>Now Playing : {{current.title}}</h2>
      <p>By {{current.artist}}</p>
      <!-- <audio controls>
        <source src="../assets/music/Until_We_Get_By.mp3" type="audio/mpeg" />
      </audio>-->
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
        <h4>Track : {{song.title}}</h4>
        <p>Artist : {{song.artist}}</p>
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
      songs: [
        {
          title: "Until We Get By",
          artist: "Small Tall Order",
          src: require("../assets/music/Until_We_Get_By.mp3")
        },
        {
          title: "Brooklyn",
          artist: "The Inventors",
          src: require("../assets/music/The_Inventors_-_12_-_Brooklyn.mp3")
        },
        {
          title: "Upbeat Party",
          artist: "Scott Holmes",
          src: require("../assets/music/Scott_Holmes_-_04_-_Upbeat_Party.mp3")
        },
        {
          title: "Dog Soldier Stand Down",
          artist: "Aglow Hollow",
          src: require("../assets/music/Aglow_Hollow_-_04_-_Dog_Soldier___Stand_Down.mp3")
        }
      ],
      player: new Audio()
      // player: ""
    };
  },
  mounted() {
    // this.player = document.querySelector("audio");
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
  methods: {
    play(song) {
      //   console.log(this.current.src);
      if (typeof song.src !== "undefined") {
        this.current = song;
        this.player.src = this.current.src;
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
      this.current = this.songs[this.index];
      this.player.src = this.current.src;
      this.isPlaying = true;
      this.player.play();
    },
    next() {
      this.index >= this.songs.length
        ? (this.index = this.songs.length - 1)
        : this.index++;
      this.current = this.songs[this.index];
      this.player.src = this.current.src;
      this.isPlaying = true;
      this.player.play();
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
