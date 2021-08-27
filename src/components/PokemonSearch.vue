<template>
    <div class="w-screen pt-10 flex flex-row items-center -mb-20 justify-center ">
        <span class="w-screen md:w-1/3 h-10 bg-gray-200 cursor-pointer border border-gray-300 text-sm rounded flex mr-6">
        <input v-on:keyup.enter="fetchData()"   type="search" name="search" placeholder="Search by name or id"
            class="flex-grow px-4 rounded-r-full font-semi-bold text-xl focus:outline-none" v-model="name">
        <i @click="fetchData()" class="fas fa-search m-3 mr-5 text-lg text-gray-700 w-4 h-4 "/>
        </span>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data(){
        return{
            name: '',
            url: 'https://pokeapi.co/api/v2/pokemon/'
        }
    },
    methods: {
        fetchData(){
            let newUrl = this.url + this.name.toLowerCase()
            axios.get(newUrl).then( res => {
                this.$router.push({ path: `/details/${res.data.id}` })
            }).catch(err => {
                console.log(err);
                alert( this.name + ' was not found')
                this.name = ""
            })
        }
    }
}
</script>
