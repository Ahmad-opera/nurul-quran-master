<template>
  <main
    class="bg-gray-50 w-full lg:flex bg-white flex-reverse fixed bottom-0 p-5"
  >
    <div class="px-4 py-2 items-center flex">
      <div class="h-12 w-12 p-3 bg-gray-100 rounded-lg"></div>
      <div class="px-4 py-2">
        <h3 class="font-bold">{{title}}</h3>
        <h3 class="font-normal text-gray-400 text-sm">{{reciter}}</h3>
      </div>
    </div>
    <div class="lg:w-8/12 justify-center flex flex-col">
      <div class="order-last lg:order-first mx-auto w-max py-2">
        <button class="px-6 py-1 text-gray-400">
          <i class="fa fa-random"></i>
        </button>
        <button class="px-2 py-1"><i class="fa fa-backward"></i></button>
        <button
        v-on:click.prevent="playing = !playing"
        :title="playing ? 'Pause' : 'Play'"
        class="w-12 h-12 bg-white shadow rounded-full">
          <i class="fa fa-play" v-if="!playing"></i>
          <i class="fa fa-pause" v-else></i>
        </button>
        <button class="px-2 py-1"><i class="fa fa-forward"></i></button>
        <!-- <button class="px-6 py-1 text-gray-400"><i class="fa fa-sync"></i></button> -->
        <button class="px-2 py-1 text-gray-400">
          <i class="fa fa-heart "></i>
        </button>
      </div>
      <div class="px-2 order-first w-full lg:order-last">
        <div class="flex justify-center items-center w-full items-center">
          <span class="flex-grow-0 px-2 text-sm">0:00</span>
          <div class="w-full object-center">
            <input
              id="track"
              class="w-full rounded-lg overflow-hidden focus:outline-none bg-gray-200 appearance-none"
              type="range"
              min=".5"
              max="100"
              step=".1"
              @click="seek"
              :value="currentSeconds * (100 / durationSeconds)"
            />
          </div>
          <span class="flex-grow-0 px-2 text-sm">0:00</span>
        </div>
      </div>
    </div>
    <audio
      :loop="looping"
      ref="audio"
      :src="file"
      v-on:timeupdate="update"
      v-on:loadeddata="load"
      v-on:pause="playing = false"
      v-on:play="playing = true"
      preload="auto"
      style="display: none"
    ></audio>
  </main>
  <!-- <div class="fixed bottom-0">
      <main class="bg-gray-50 w-full lg:flex flex-reverse fixed bottom-0 p-5">
            <div class="px-4 py-2 bg-white items-center flex">
                <div class="h-12 w-12 p-3 bg-gray-100 rounded-lg"></div>
                <div class="px-4 py-2">
                    <h3 class="font-bold">Title goes here</h3>
                    <h3 class="font-normal text-gray-400 text-sm">Title goes here</h3>
                </div>
            </div>
            <div class="justify-center flex flex-col">
                <div class="order-last lg:order-first mx-auto w-max py-2">
                    <button class="px-6 py-1 text-gray-400"><i class="fa fa-random"></i>Shuffle</button>
                    <button class="px-2 py-1"><i class="fa fa-backward"></i>Backward</button>
                    <div id="play">
        <a
          v-on:click.prevent="playing = !playing"
          :title="playing ? 'Pause' : 'Play'"
          href="#" class="w-56 "
        >
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" class="w-12">
            <path
              v-if="!playing"
              fill="currentColor"
              d="M15,10.001c0,0.299-0.305,0.514-0.305,0.514l-8.561,5.303C5.51,16.227,5,15.924,5,15.149V4.852c0-0.777,0.51-1.078,1.135-0.67l8.561,5.305C14.695,9.487,15,9.702,15,10.001z"
            />
            <path
              v-else
              fill="currentColor"
              d="M15,3h-2c-0.553,0-1,0.048-1,0.6v12.8c0,0.552,0.447,0.6,1,0.6h2c0.553,0,1-0.048,1-0.6V3.6C16,3.048,15.553,3,15,3z M7,3H5C4.447,3,4,3.048,4,3.6v12.8C4,16.952,4.447,17,5,17h2c0.553,0,1-0.048,1-0.6V3.6C8,3.048,7.553,3,7,3z"
            />
          </svg>
        </a>
      </div>
                    <button class="px-2 py-1"><i class="fa fa-forward"></i>Forward</button>
                     <button class="px-6 py-1 text-gray-400"><i class="fa fa-sync"></i></button>
                    <button class="px-2 py-1 text-gray-400"><i class="fa fa-volume-up"></i>Volume</button>
            </div>
            <div class="px-2 order-first lg:order-last">
                <div class="flex justify-center items-center">
                    <span class="flex-grow-0 px-2 text-sm">{{Math.floor(currentSeconds / 60) }}:{{Math.floor(Number(currentSeconds / 60).toString().split('.')[1] * 60) }}</span>
                    <div class="">
                        <input id="track" class="rounded-lg overflow-hidden focus:outline-none bg-gray-200 appearance-none" type="range" min=".5" max="100" step=".1" @click="seek" :value="currentSeconds * (100 / durationSeconds)" />
                    </div>
                    <span class="flex-grow-0 px-2 text-sm">0:00</span>
                </div>
            </div>
        </div>
        <div class="px-6">
        </div>
<audio
      :loop="looping"
      ref="audio"
      :src="file"
      v-on:timeupdate="update"
      v-on:loadeddata="load"
      v-on:pause="playing = false"
      v-on:play="playing = true"
      preload="auto"
      style="display: none"
    ></audio>
        </main>
  </div> -->
</template>

<script>
export default {
  name: 'Player',
  props: {
    file: {
      type: String,
      default: null
    },
    title: {
      type: String,
      default: null
    },
    reciter: {
      type: String,
      default: null
    },
    playList: {
      type: Array,
      default: null
    },
    autoPlay: {
      type: Boolean,
      default: false
    },
    loop: {
      type: Boolean,
      default: false
    }
  },
  data: () => ({
    currentSeconds: 0,
    durationSeconds: 0,
    loaded: false,
    looping: false,
    playing: false,
    previousVolume: 35,
    showVolume: false,
    volume: 100,
    currentTrack: {}
  }),
  computed: {
    muted () {
      return this.volume / 100 === 0
    },
    percentComplete () {
      return parseInt((this.currentSeconds / this.durationSeconds) * 100)
    }
  },
  filters: {
    convertTimeHHMMSS (val) {
      const hhmmss = new Date(val * 1000).toISOString().substr(11, 8)

      return hhmmss.indexOf('00:') === 0 ? hhmmss.substr(3) : hhmmss
    }
  },
  watch: {
    playing (value) {
      if (value) {
        return this.$refs.audio.play()
      }
      this.$refs.audio.pause()
    },
    volume (value) {
      this.$refs.audio.volume = this.volume / 100
    }
  },
  methods: {
    download () {
      this.stop()
      window.open(this.file, 'download')
    },
    load () {
      if (this.$refs.audio.readyState >= 2) {
        this.loaded = true
        this.durationSeconds = parseInt(this.$refs.audio.duration)

        return (this.playing = this.autoPlay)
      }

      throw new Error('Failed to load sound file.')
    },
    mute () {
      if (this.muted) {
        return (this.volume = this.previousVolume)
      }
      this.previousVolume = this.volume
      this.volume = 0
    },
    seek (e) {
      if (!this.loaded) return
      const el = e.target.getBoundingClientRect()
      const seekPos = (e.clientX - el.left) / el.width

      this.$refs.audio.currentTime = parseInt(
        this.$refs.audio.duration * seekPos
      )
    },
    stop () {
      this.playing = false
      this.$refs.audio.currentTime = 0
    },
    update (e) {
      this.currentSeconds = parseInt(this.$refs.audio.currentTime)
    }
  },
  created () {
    this.looping = this.loop
  }
}
</script>

<style>
body {
  font-family: "Quicksand";
}
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  input[type="range"]::-webkit-slider-runnable-track::before {
    background: greenyellow;
    height: 5px;
    width: 70vw;
  }
  input[type="range"]::-webkit-slider-thumb {
    width: 15px;
    -webkit-appearance: none;
    appearance: none;
    height: 5px;
    cursor: pointer;
    background: rgb(26, 209, 81);
    box-shadow: 0 0 40px #f0f0ef;
  }
}
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  input[type="range"]::-webkit-slider-runnable-track::before {
    background: greenyellow;
    height: 5px;
    width: 300px;
  }
  input[type="range"]::-webkit-slider-thumb {
    width: 15px;
    -webkit-appearance: none;
    appearance: none;
    height: 5px;
    cursor: pointer;
    background: rgb(26, 209, 81);
    box-shadow: 0 0 40px #f0f0ef;
  }
}
</style>
