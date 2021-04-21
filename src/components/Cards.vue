<template>
  <div class="cards">
    <div class="card" v-for="starter in starters">
      <div class="title">{{starter.name}}</div>
      <div class="content">
        <img :src="starter.sprites" alt="{{starter.name}}">
      </div>
      <div class="description">
        <div v-for="elem in starter.type">
          {{elem}}
        </div>
      </div>
    </div>
  </div>

</template>

<script>
const baseUrl = 'https://pokeapi.co/api/v2'
const ids = [1, 4, 7]
export default {
  name: "Card",
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
.card{
  border: 1px solid silver;
  border-radius: 8px;
  max-width: 200px;
  margin: 0 5px;
  cursor: pointer;
  box-shadow: 0px 1px 3px darkgray;
  transition: 0.2s;
}
img{
  width: 100%;
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
