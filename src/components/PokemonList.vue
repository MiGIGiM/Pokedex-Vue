<template>
        <article class="min-h-screen flex flex-col items-center justify-center">
            <pokemon-search/>
            <article class="grid xl:grid-cols-5 md:grid-cols-3 sm:grid-cols-2 gap-10">
                <pokemon-card v-for="pokemon in pokemonArray" :key="pokemon" :pokemon="pokemon"/>
            </article>
            <div class="flex justify-center my-4">
                <i @click="previous" class="fas fa-chevron-circle-left mx-8 text-3xl self-center hover:text-gray-300 cursor-pointer text-white" ></i>
            <i @click="next" class="fas fa-chevron-circle-right text-3xl mx-8 self-center cursor-pointer text-white hover:text-gray-300"></i>
            </div>
        </article>
</template>

<script>
import PokemonCard from "./PokemonCard.vue";
import PokemonSearch from './PokemonSearch.vue'
import axios from "axios";
export default {
    components: {
        PokemonCard,
        PokemonSearch
    },
    data() {
        return {
            pokemonArray: [],
            selectedPokemon: [],
            apiUrl: 'https://pokeapi.co/api/v2/pokemon/?limit=10',
            nextUrl: '',
            currentUrl: '',
            previousUrl : '',
        };
    },
    props: ['pastUrl'],
    methods: {
        fetchData(){
            axios.get(this.apiUrl).then( res => {
                this.nextUrl = res.data.next;
                this.previousUrl = res.data.previous;
                res.data.results.forEach(pkmn => {
                    this.fetchPokemon(pkmn)
                })
            }).catch(err => {
                console.log(err);
            })
        },
        fetchPokemon(pokemon){
            axios.get(pokemon.url).then( res => {
                let pokemon = {
                    img: res.data.sprites.front_default,
                    id: res.data.id,
                    name: res.data.name,
                    types: res.data.types
                };

                this.pokemonArray.push(pokemon);
                this.pokemonArray.sort(function(a, b) {
                    return a.id - b.id;
                });
            }).catch((err) => {
                console.log(err);
            });
        },
        next(){
            if(this.nextUrl === null)
                alert("You already reached the last page")
            else{
                this.pokemonArray = []
                this.apiUrl = this.nextUrl
                this.fetchData()
            }
        },
        previous(){
            if(this.previousUrl === null)
                alert("Can't go back any further")
            else{
                this.pokemonArray = []
                this.apiUrl = this.previousUrl
                this.fetchData()
            }
        },
    },
    created() {
        this.currentUrl = this.apiUrl
        this.fetchData()
    },
};
</script>