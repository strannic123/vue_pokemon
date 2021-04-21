<template>
  <div class="cards">
    <Card v-for="starter in starters">

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

</template>

<script>
import Card from './Card'

const baseUrl = 'https://pokeapi.co/api/v2'
const ids = [1, 4, 7]
export default {
  name: "Cards",
  components: {
    Card
  },
  data(){
    return {
      starters: []
    }
  },
  methods: {
   async fetchData(){
     const responses = await Promise.all(ids.map(id =>window.fetch(`${baseUrl}/pokemon/${id}`)))
     const data = await Promise.all(responses.map(res => res.json()))
     this.starters = data.map(datum => ({
         name: datum.name,
         sprites: datum.sprites.other['official-artwork'].front_default,
         type: datum.types.map(type =>  type.type.name)
     }))
   }
  },
  created() {
    this.fetchData()
  }

}
</script>

<style scoped>
.cards{
  display: flex;
}
img{
  width: 100%;
}

</style>
