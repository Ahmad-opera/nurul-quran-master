<template>
  <div class="pt-5 bg-gray-50" v-s >
    <h1 class="font-semibold text-2xl p-2">Top Albums</h1>
    <div class="grid grid-cols-5 gap-5 p-5">

      <router-link
      v-for="topAlbum in topAlbums"
        :key="topAlbum.id" :to="'/album/' + topAlbum.id" >
      <div
        class="px-4 py-3 w-18 bg-white rounded-2xl shadow"
      >
        <img :src="topAlbum.image" alt="hello" class="w-24 rounded-full mx-auto" />
        <h5 class="text-center">{{ topAlbum.name }}</h5>
      </div>
      </router-link>
    </div>
    <div class="px-8 py-4 bg-gray-50 mt-3 overflow-scroll">
      <h1 class="font-semibold text-2xl">Top Tracks</h1>
      <ul>
        <li
          class="px-4 py-2 hover:shadow"
          v-for="topSong in topSongs"
          :data-id="topSong.id"
          :key="topSong.id"
          @click="setPlay(topSong.id)"
        >
          {{ topSong.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  components: {},
  data () {
    return {
      topAlbums: null,
      topSongs: null
    }
  },
  methods: {
    setPlay (value) {
      const fetchSong = async (dataVal) => {
        const songData = await fetch(`http://nqapp.mynurulquran.com/index.php/api/listenSong?id=${dataVal}`)
          .then((res) => res.json())
          .then((data) => {
            // console.log(data)
            return data.data
          })
          .catch((err) => console.log(err, 'Error Fetching Data'))
        this.$emit('clickedsong', songData)
      }
      fetchSong(value)
    },
    fetchTop () {
      const fetchData = async () => {
        const topSongs = await fetch(
          'http://nqapp.mynurulquran.com/index.php/api/album?page=1'
        )
          .then((res) => res.json())
          .then((data) => {
            return data.data.splice(0, 5)
          })
          .catch((err) => console.log(err, 'Error Fetching Data'))
        this.topAlbums = topSongs
      }
      fetchData()
    },
    fetchSongs () {
      const fetchData = async () => {
        const fetchSongs = await fetch(
          'http://nqapp.mynurulquran.com/index.php/api/topSong?page=1'
        )
          .then((res) => res.json())
          .then((data) => {
            return data.data
          })
          .catch((err) => console.log(err, 'Error Fetching Data'))
        this.topSongs = fetchSongs
      }
      fetchData()
    }
  },
  mounted () {
    this.fetchTop()
    this.fetchSongs()
  }
}
</script>
