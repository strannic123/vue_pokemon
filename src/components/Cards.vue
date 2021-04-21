<template>
  <div class="cards">
    <Card
        v-for="starter in starters"
        @click="fetchEvolutions(starter)"
        :class="{opace: selectedId && selectedId !== starter.id}"
        class="card"
    >

      <template v-slot:title>
        {{starter.name}}
      </template>

      <template v-slot:content>
        <img :src="starter.sprites" alt="{{starter.name}}">
      </template>

      <template v-slot:description>
        <div v-for="elem in starter.type">
          {{elem}}
        </div>
      </template>

    </Card>
  </div>

  <div class="cards">
    <Card v-for="evolut in evolutions">

      <template v-slot:title>
        {{evolut.name}}
      </template>

      <template v-slot:content>
        <img :src="evolut.sprites" alt="{{evolut.name}}">
      </template>

      <template v-slot:description>
        <div v-for="elem in evolut.type">
          {{elem}}
        </div>
      </template>

    </Card>
  </div>

</template>

<script>
import Card from './Card'

const baseUrl = 'https://pokeapi.co/api/v2'
const STARTER_IDS = [1, 4, 7]
export default {
  name: "Cards",
  components: {
    Card
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
.cards{
  display: flex;
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
