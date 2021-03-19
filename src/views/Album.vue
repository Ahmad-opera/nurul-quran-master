<template>
  <div class="pt-24">
    <div class="flex flex-col gap-2 p-5">
      <div
        class="px-4 py-3 w-18 bg-white rounded-2xl shadow"
        v-for="song in songs"
        :key="song.id"
        @click="setPlay(song.id)"
        >{{ song.name }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Album',
  data () {
    return {
      songs: null
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
    fetchSongs (album) {
      const fetchData = async (album) => {
        const songs = await fetch(
          `http://nqapp.mynurulquran.com/index.php/api/songAlbum?albumId=${album}&page=1`
        )
          .then((res) => res.json())
          .then((data) => {
            return data.data
          })
          .catch((err) => console.log(err, 'Error Fetching Data'))
        this.songs = songs
      }
      fetchData(album)
    }
  },
  mounted () {
    this.fetchSongs(this.$route.params.id)
  }
}
</script>

<style>

</style>
