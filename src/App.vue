<template>
  <div class="bg-gray-100 transition duration-300 h-screen">
    <button
      class="my-auto rounded-full shadow px-2 absolute left-0 z-40 w-10 h-10 focus:outline-none m-3 bg-white justify-center"
      @click="paneShown = !paneShown"
    >
      <i class="fa fa-chevron-left" v-if="paneShown"></i
      ><i class="fa fa-chevron-right" v-else></i>
    </button>
    <h1 v-for="item in playList" :key="item.title">{{item.title}}</h1>
    <div
      id="nav"
      class="fixed left-0 bg-green-200"
      :class="paneShown ? 'h-screen w-2/12' : 'w-0 h-screen'"
    >
      <div class="pt-10 px-4 py-2 block" v-if="paneShown">
        <router-link to="/" class="block">Home</router-link>
        <router-link to="/about" class="block">About</router-link>
      </div>
    </div>

    <router-view
      class="bg-blue-100 h-screen right-0 fixed"
      :class="paneShown ? 'w-10/12 pl-12' : 'w-full pl-12'"
    />
  </div>
  <!-- <AudioPlayer file="http://nqapp.nurulquran.com//audios//Quran//Tafseer//Detailed-Tafseer//Nq-2013//Juz-01//L01-AlFateha-1-7//NQ13_L1_Taf-B-Fatihah.mp3" /> -->
  <Player :playList="playList"
    file="https://upload.wikimedia.org/wikipedia/commons/d/d6/Louis-Emmanuel_Jadin_-_Nocturne_No._3_in_G_minor_-_2._Allegro_molto.ogg"
  />
</template>

<script>
import Player from './components/Player'
// import AudioPlayer from './components/AudioPlayer'
export default {
  name: 'App',
  components: {
    Player
  },
  data () {
    return {
      currentTrack: {},
      paneShown: true,
      playList: { name: 'Ahmad' }
    }
  },
  mounted () {
    fetch('http://nqapp.mynurulquran.com/index.php/api/topSong?page=1').then(res => res.json()).then(data => {
      // console.log(data)
      this.playList = data.data
    })
  },
  beforeUpdate () {
    if (this.playtList !== {}) console.log(this.playList)
  }
}
</script>

<style>
@import './assets/tailwind.min.css';
</style>
