<template>
  <div class="card">
    <div class="title">Title</div>
    <div class="content">content</div>
    <div class="description">description</div>
  </div>
</template>

<script>
const baseUrl = 'https://pokeapi.co/api/v2'
const ids = [1, 4, 7]
export default {
  name: "Card",
  data(){
    return {
      pokemon: null
    }
  },
  methods: {
   async fetchData(){
     const responses = await Promise.all(ids.map(id =>window.fetch(`${baseUrl}/pokemon/${id}`)))
     const data = await Promise.all(responses.map(res => res.json()))
     const pokemon = data.map(datum => ({
         name: datum.name,
         sprites: datum.sprites.other['official-artwork'].front_default,
         type: datum.types.map(type => ({name: type.type.name}))
     }))
     console.log(pokemon)
   }
  },
  created() {
    this.fetchData()
  }

}
</script>

<style scoped>
.card{
  border: 1px solid silver;
  border-radius: 8px;
  max-width: 200px;
  margin: 0 5px;
  cursor: pointer;
  box-shadow: 0px 1px 3px darkgray;
  transition: 0.2s;
}
.title, .content, .description{
  padding: 16px;
  text-transform: capitalize;
  text-align: center;
}
.title, .content{
  border-bottom: 1px solid silver;
}
.title{
  font-size: 1.25em;
}
.card:hover{
  transition: 0.2s;
  box-shadow: 0px 1px 9px darkgray;
}
</style>
