<template>
    <div class="w-screen flex flex-row items-center p-5 px-2 justify-center ">
        <span class="md:w-1/3 sm:w-1/4 h-10 bg-gray-200 cursor-pointer border border-gray-300 text-sm rounded flex mr-6">
        <input v-on:keyup.enter="fetchData()"   type="search" name="search" placeholder="Search by name or id"
            class="flex-grow px-4 rounded-r-full font-semi-bold text-xl focus:outline-none" v-model="name">
        <button @click="fetchData()" class=" m-3 mr-5 w-4 h-4 ">
            <i class="fas fa-search text-lg text-gray-700 "></i>
        </button>
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
            if(this.name == '') alert('Please enter a pokemon name or id')
            else{
                let newUrl = this.url + this.name.toLowerCase()
                axios.get(newUrl).then( res => {
                    this.$router.push({ path: `/details/${res.data.id}` })
                }).catch(err => {
                    console.log(err);
                    alert( this.name + ' was not found')
                    this.name = ""
                })
                }
        },
    }
}
</script>
