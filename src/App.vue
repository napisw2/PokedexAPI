<template>

<div id="app">
  <div class="colum is-half is-offset-one-quarter">
    <img src="./assets/poke4.jpg">
    <hr>
    <h4 class="is-size-4">Pokedex</h4>
    <div class="container is-max-desktop">
      <input id="busca1" type="text" placeholder="Buscar pokemon pelo nome" v-model="busca"  class="input is-rounded"/> 
      <button  class="button is-fullwidth is-success" id="BtnEnter" @click="buscar">buscar</button>
    </div>
    <div v-for="(poke,index) in filteredPokemons" :key="poke.url">
      <PokemonComp :name="poke.name" :url="poke.url" :num="index+1" /> 
    </div>
  </div>

</div>

</template>

<script>
import axios from 'axios';
import PokemonComp from './components/PokemonComp.vue'

export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      busca: '',
      filteredPokemons: []
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => { //pegando a lista de pokemons do site
      console.log("Pegou a lista de Pokemon");             
      this.pokemons = res.data.results;  
      this.filteredPokemons = res.data.results
    });
  },
  mounted : function(){
    const inputEnter = document.getElementById('busca1');
    console.log(inputEnter)
    inputEnter.addEventListener('keypress', function(e){
      var key = e.which || e.keyCode;
      console.log(e);     
      if (key == 13) { 
        document.getElementById('BtnEnter').click();
        
      }
    }); 
  },
  components: {
    PokemonComp
  },
  methods: {
    buscar: function(){
      console.log(this.pokemons)
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca.toLowerCase());
      }
    },
    
  }
 
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 2%;
  margin-right: 2%;
} 
#BtnEnter{
 
 margin-top: 2%;

}
</style>
