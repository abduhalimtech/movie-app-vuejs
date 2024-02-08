<template>
    <div class="movie-add-form">
        <h3>Yangi kino qo'shish</h3>
        <form class="add-form d-flex" @submit.prevent>
            <Input class="new-movie-level" placeholder="Qanday kino?" v-model="name" />
            <Input type="number" class="new-movie-level" placeholder="Necha marotaba ko`rilgan" v-model="viewers" />
            <PrimaryButton type="submit" class="btn btn-outline-dark" @click="addMovie">Qo'shish</PrimaryButton>
        </form>
    </div>
</template>
<script>
    import PrimaryButton from '@/ui-components/PrimaryButton.vue'
export default {
    data() {
        return {
            name: '',
            viewers: '',
        }
    },
    components:{
        PrimaryButton,
    },
    methods: {
        addMovie() {
            if (!this.name || !this.viewers) return
            const newMovie = {
                name: this.name,
                viewers: this.viewers,
                favourite: false,
                like: false,
            }
            this.$emit('createMovie', newMovie)
            this.name = ''
            this.viewers = ''
        },
    },
}
</script>
<style scoped>
    .movie-add-form{
        margin-bottom: 1rem;
        padding: 1.5rem;
        background-color: #fcfaf5;
        border-radius: 4px;
        box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
    }
</style>