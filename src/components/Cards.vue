<template>

  <PokemonCards
      :pokemons="starters"
      @clickPokemon="fetchEvolutions"
      :selectedId="selectedId"
  />

  <PokemonCards
      :pokemons="evolutions"
  />

</template>

<script>
import Card from './Card'
import PokemonCards from "./PokemonCards";

const baseUrl = 'https://pokeapi.co/api/v2'
const STARTER_IDS = [1, 4, 7]
export default {
  name: "Cards",
  components: {
    Card,
    PokemonCards
  },
  data(){
    return {
      starters: [],
      evolutions: [],
      selectedId: null
    }
  },
  methods: {
    async fetchEvolutions(pokemon){
      this.selectedId = pokemon.id
      this.evolutions = await this.fetchData([pokemon.id+1, pokemon.id+2])
    },
   async fetchData(ids){
     const responses = await Promise.all(ids.map(id =>window.fetch(`${baseUrl}/pokemon/${id}`)))
     const data = await Promise.all(responses.map(res => res.json()))
      return data.map(datum => ({
       id: datum.id,
       name: datum.name,
       sprites: datum.sprites.other['official-artwork'].front_default,
       type: datum.types.map(type =>  type.type.name)
     }))
   }
  },
  async created() {
    this.starters = await this.fetchData(STARTER_IDS)
  }

}
</script>

<style scoped>


</style>
