<template>
    
    <Box>
        <p class="fs-3 text-uppercase">Barcha kinolar soni: {{ AllMoviesCount }}</p>
        <p class="fs-4 text-uppercase">Ko'rilgan kinolar soni: {{ FavouriteMoviesCount }}</p>
    </Box>
</template>

<script>
    export default{
        props:{
            AllMoviesCount:{
                type: Number,
                required: true,
            },
            FavouriteMoviesCount:{
                type: Number,
                required: true,
            },
        },
    }
</script>

<style scoped>
</style>