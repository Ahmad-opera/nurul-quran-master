<template>
  <div class="px-4 py-2 bg-white">
    <ul class="flex">
      <li class="px-4 py-2" v-for="category in categories" :key="category.id"> {{ category.name }} </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'NavBar',
  data () {
    return {
      categories: null
    }
  },
  methods: {
    getCategories () {
      const fetchData = async () => {
        const catgs = await fetch('http://nqapp.mynurulquran.com/index.php/api/category?page=1').then(res => res.json()).then(data => { return data.data }).catch(err => console.log(err, 'Error Fetching Data'))
        this.categories = catgs
      }
      fetchData()
    }
  },
  mounted () {
    this.getCategories()
  },
  created () {
    this.getCategories()
  },
  beforeUpdate () {
    console.log(this.categories)
  }
}
</script>

<style>

</style>
