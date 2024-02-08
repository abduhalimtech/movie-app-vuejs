<template>
  <div class="app font-monospace">
    <div class="content">
      <!-- <PrimaryButton @click="fetchMovies">fetch</PrimaryButton> -->
      <AppInfo :AllMoviesCount="movies.length" :FavouriteMoviesCount="movies.filter(c=>c.favourite).length"/>
      <div class="search-panel">
        <AppSearchPanel :onUpdateTermHandler = "onUpdateTermHandler"/>
        <AppFilter :OnUpdateFilterHandler= "OnUpdateFilterHandler" :filterName ="filter" />
      </div>
      <Box v-if="!movies.length && !isLoading">
        <p class="text-center fs-3 text-danger">  Kinolar yo`q </p>
      </Box>
      <Box v-else-if="isLoading">
        <Loader class="d-flex justify-content-center"/>
      </Box>
      <AppMovieList v-else :movies="onFilterHandler(onSearchHandler(movies, term), filter)" @onToggle = "onToggleHandler" @onDelete = "onDeleteHandler"/>
  
      <Box>
        <Pagination :totalPages="totalPages" :page="page" @page-changed="changePageHandler" class="d-flex justify-content-center"/>
      </Box>
      
      <AppMovieAddForm @createMovie = 'createMovie' />
    </div>
  </div>
</template>

<script>
  import AppInfo from "@/components/app-info/AppInfo.vue"
  import AppSearchPanel from "@/components/app-search-panel/AppSearchPanel.vue"
  import AppFilter from "@/components/app-filter/AppFilter.vue";
  import AppMovieList from "@/components/app-movie-list/AppMovieList.vue";
  import AppMovieAddForm from "@/components/app-movie-add-form/AppMovieAddForm.vue";
  import axios from "axios"
    export default{
          components: {
          AppInfo,
          AppSearchPanel,
          AppFilter,
          AppMovieList,
          AppMovieAddForm
        },
        data() {
          return {
              movies: [],
              term:'',
              filter: 'all',
              isLoading : false,
              limit: 10,
              page: 1,
              totalPages: 0,
          }
        },
        methods: {
          async createMovie(item){
            try {
              const response = await axios.post(`https://jsonplaceholder.typicode.com/posts`, item)
              this.movies.push(item)
            } catch (error) {
              alert(error.message)
            }
          },
          onToggleHandler({id, prop}){
            this.movies  = this.movies.map(item => {
                if (item.id == id) {
                  return {...item, [prop] : !item[prop]}
                }
                return item
              })
          },
          async onDeleteHandler(id){
            try {
              const response = await axios.delete(`https://jsonplaceholder.typicode.com/posts/${id}`)
              console.log(response)
              this.movies = this.movies.filter(c => c.id != id)
            } catch (error) {
              alert(error.message)
            }
          },
          onSearchHandler(arr, term){
            if(term.length==0){
              return arr
            }
            return arr.filter(c=> c.name.toLowerCase().indexOf(term) >-1 )

          },
          onUpdateTermHandler(term){
            this.term = term
          },
          onFilterHandler(arr, filter){
            
            switch (filter) {
              case 'popular':
                return arr.filter(c=>c.like)
              case 'mostViewers':
                return arr.filter(c=>c.viewers > 500)
              default :
                return arr
            }
          },
          OnUpdateFilterHandler(filter){
            this.filter = filter
          },
          async fetchMovies(){
            try {
              this.isLoading = true
              const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                params:{
                  _limit : this.limit,
                  _page: this.page,
                },
              })
              const newArr = response.data.map(item => ({
                  name: item.title,
                  viewers: item.id * 13,
                  favourite: false,
                  like: false,
                  id : item.id,
              }))
              this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
              this.movies = newArr
            } catch (error) {
              alert(error.message)
            }finally{
              this.isLoading = false
            }
          },
          changePageHandler(newPage) {
            this.page = newPage;
            this.fetchMovies();
          }
        },
        watch: {
          limit(newLimit) {
            this.page = 1;
            this.fetchMovies();
          }
        },
        mounted() {
          this.fetchMovies()
        },
    }
</script>

<style>
  .app{
    height: 100vh;
    color: #000;
  }
  .content{
    width: 1000px;
    min-height: 700px;
    background-color: #fff;
    margin: 0 auto;
    padding: 5rem 0;
  }
  .search-panel{
    margin-top: 2rem;
    padding: 1.5rem;
    background-color: #fcfaf5;
    border-radius: 4px;
    box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
  }
</style>