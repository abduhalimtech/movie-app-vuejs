<template>
    <ul  class="movie-list list-group">
        <AppMovieListItem v-for="movie in movies" v-bind:movie="movie" :key = "movie.id" @onToggle = "$emit('onToggle', $event)" @onDelete = "$emit('onDelete', $event)" />
    </ul>
</template>
<script>
import AppMovieListItem from "@/components/app-movie-list-item/AppMovieListItem.vue";
export default {
    components: {
        AppMovieListItem
    },
    props:{
        movies:{
            type: Array,
            requried : true,
        },
    },

}
</script>
<style scoped>
    .movie-list{
        margin-top: 2rem;
        padding: 1.5rem;
        background-color: #fcfaf5;
        border-radius: 4px;
        box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
    }
</style>