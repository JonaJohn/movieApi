<template>
  <div>
    <div class="container">
      <h1>Film eingeben:</h1>
      <input v-model="input" type="text" placeholder="write a moviename">
      <button @click="searchForMovie"> Search </button>
    </div>
    <div class="info">
      <h1>All information fetched:</h1>
      <p>The Film Id: {{ filmId }}</p>
      <p>The filmLength: {{ filmLength }} min</p>
      <img :src="filmImg" alt="huch das hat nicht geklappt">
    </div>
  </div>
</template>

<script>

export default {
  name: 'UserInput',
  data () {
    return {
      myTMDBkey: '954fcaaba4f9cfd1fdf663fa0d48540d',
      counter: 0,
      input: '',
      filmId: 0,
      filmLength: 0,
      filmImg: ''
    }
  },
  methods: {
    async searchForMovie () {
      console.log('hier kommt der input=' + this.input)
      if (this.input !== '') {
        const url = 'https://api.themoviedb.org/3/search/movie?api_key=954fcaaba4f9cfd1fdf663fa0d48540d&language=en-US&query=' + this.input + '&page=1&include_adult=false'
        console.log('url des Filmes: ' + url)
        const respond = await fetch(url)// speichert alle infos in res = respond
        const { results } = await respond.json()
        console.log(results)
        this.filmId = results[0].id
        console.log(this.filmId)

        const idUrl = 'https://api.themoviedb.org/3/movie/' + this.filmId + '?api_key=954fcaaba4f9cfd1fdf663fa0d48540d&language=en-US'
        console.log(idUrl)
        // https://api.themoviedb.org/3/movie/106646?api_key=954fcaaba4f9cfd1fdf663fa0d48540d&language=en-US
        const res = await fetch(idUrl)
        const ergebnisse = await res.json()
        console.log(ergebnisse)
        this.filmLength = ergebnisse.runtime
        const imgInfo = ergebnisse.poster_path

        this.filmImg = 'https://image.tmdb.org/t/p/w500/' + imgInfo
      }
    }
  }
}

</script>

<style>
  .container{
    display: flex;
    align-items: center;
    justify-content: center;
    border: solid black;
  }
  .info{
    margin-left: 30%;
    margin-right: 30%;
    border: 1px solid red;
    justify-content: center;
  }
</style>
