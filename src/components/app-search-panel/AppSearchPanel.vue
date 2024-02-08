<template>
    <Input type="text" class="form-control search-input"   placeholder="Kinolarni qidirish" v-model="term" @input="changeHandler" />
</template>
<script>
export default {
    props:{
        onUpdateTermHandler:{
            type: Function,
            required: true,
        },
    },
    data() {
        return {
            term: '',
        }
    },
    methods: {
        changeHandler(e){
            this.term = e.target.value
            this.onUpdateTermHandler(this.term)
        }
    },

}
</script>
<style scoped>
    .search-input{
        margin-bottom: 1rem;
        padding: 1.5rem;
        background-color: #fcfaf5;
        border-radius: 4px;
        box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
    }
</style>