<template>
  <div class="cards">
    <Card
        v-for="pokemon in pokemons"
        @click="click(pokemon)"
        :class="{opace: selectedId && selectedId !== pokemon.id}"
        class="card"
    >

      <template v-slot:title>
        {{pokemon.name}} #{{pokemon.id}}
      </template>

      <template v-slot:content>
        <img :src="pokemon.sprites" alt="{{pokemon.name}}">
      </template>

      <template v-slot:description>
        <div v-for="elem in pokemon.type">
          {{elem}}
        </div>
      </template>

    </Card>
  </div>



</template>

<script>
import Card from "./Card";
export default {
  name: "PokemonCards",
  components: {
    Card
  },
  props: {
    pokemons: {
      type: Array
    },
    selectedId: {
      type: Number
    }

  },
  methods: {
      click(pokemon){
        this.$emit('clickPokemon', pokemon)
      }
    },

}
</script>

<style scoped>
.cards{
  display: flex;
  justify-content: center;
}
.card:hover{
  opacity: 1.0;
}
.opace{
  opacity: 0.5;
}
img{
  width: 100%;
}

</style>
