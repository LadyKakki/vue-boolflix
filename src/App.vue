<template>
  <div id="app">

    <header class="header d-flex justify-content-between">
    <h1 class="display-6">Boolflix</h1>
    <search-bar @performSearch="search"/>
    </header>
    
  

    <main class="main">
      
      <loader-component v-if="loading"/>
      <loader-component v-if="loadingSeries"/>

      <grid-list :items="movies" title="Movies" :loader="loading"/>
      <grid-list :items="series" title="Series" :loader="loadingSeries"/>

    </main>

  
  </div>
</template>

<script>
import axios from 'axios'
import GridList from './components/GridList.vue'
import SearchBar from './components/SearchBar.vue'
import LoaderComponent from './components/LoaderComponent.vue'



export default {
  name: 'App',
  components: {
    SearchBar,
    GridList,
    LoaderComponent,
  },data(){
    return{
      apiKey:'7bb93464ea396b5c4394f00e6170ccea',
      apiPath:'https://api.themoviedb.org/3/search/',
      apiPathActor:'https://api.themoviedb.org/3/person/',
      movies:[],
      series:[],
      actors:[],
      loading:false,
      loadingSeries:false,
      

    }
  },

    
  methods:{
    getMovies(queryParams){
      this.loading = true;
      axios.get(this.apiPath + 'movie',queryParams).then((res)=>{
        // console.log(res.data.results)
        this.movies = res.data.results;
        this.loading = false;


      }).catch((error)=>{
        console.log(error)
      })
    },getSeries(queryParams){
      this.loading = true;
      axios.get(this.apiPath + 'tv',queryParams).then((res)=>{
        // console.log(res.data.results)
        this.series = res.data.results;
        this.loadingSeries = false;


      }).catch((error)=>{
        console.log(error)
      })

    },getActor(queryParams){
      // https://api.themoviedb.org/3/person/changes?api_key=7bb93464ea396b5c4394f00e6170ccea&page=1
      axios.get(this.apiPathActor + 'changes',queryParams).then((res)=>{
        // console.log(res.data.results)
        this.actors = res.data.results;
        


      }).catch((error)=>{
        console.log(error)
      })
      

    },
    search(text){
      const queryParams = {
        params:{
          api_key:this.apiKey,
          query:text,

        }
      }
      this.loading = true;
      this.loadingSeries = true;
      this.getMovies(queryParams)
      this.getSeries(queryParams)
      this.getActor(queryParams)


    },
  }
  
}
</script>

<style lang="scss">
@import './styles/general.scss';
.header{
  background-color: $bg-main;
}

.main{
  background-color: $bg-navbar;
  height: 1000vh;
}
h1{
  color: $tt-color;
}



</style>
