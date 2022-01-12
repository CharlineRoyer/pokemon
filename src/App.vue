<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <Grid></Grid>

    <div :key="poke.nomFrancais" v-for="poke in pokemon">
      
      <img :src="poke.image">
      <h1>{{poke.nomFrancais}}</h1>
      <p>{{poke.elementType}}</p>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Grid from './components/grillePokemon/Grid.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    HelloWorld,
    Grid
    
  },
  data(){
    return{
      pokemon : [],
      
    }
  },

  mounted(){
     axios
    .get('https://pokeapi.co/api/v2/pokemon?limit=3')
    .then((response) =>{
        console.log(response.data.results)
        let responsePokemon = response.data.results;

      for(let i = 0; i<responsePokemon.length; i++){
        let pokemon = {}
        pokemon.name = responsePokemon[i].name;

        axios
        .get(`https://pokeapi.co/api/v2/pokemon/${pokemon.name}`)
        .then((response2)=>{
          pokemon.image = response2.data.sprites.front_default
          pokemon.id = response2.data.id
          pokemon.elementType = response2.data.types[0].type.name
          //  console.log(response2.data.types[0].type.name)
       


         axios
            .get(`https://pokeapi.co/api/v2/pokemon-species/${pokemon.id}`)
            .then((response3)=>{

                // console.log(response3.data.names[4].name)

              pokemon.nomFrancais= response3.data.names[4].name
              
         

          this.pokemon.push(pokemon)
         
          })

        })
        
      }
    });
  } 
 
};




</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
