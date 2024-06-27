<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img class="image-border" src="./assets/600x150.png" alt="Imagem de template com 600px X 150px">
      
      <h1 class="is-size-3">Pokedex</h1>

      <div class="field is-grouped">
        <p class="control is-expanded">
          <input type="text" placeholder="Buscar pokemon pelo nome" v-model="busca" class="input is-rounded">
        </p>
        <p class="control">
          <button class="button is-info is-rounded" @click="buscar">
            Buscar
          </button>
        </p>
      </div>

      <hr>

      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <PokemonList :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonList from './components/Pokemon.vue';

export default {
  name: 'App',
  data() {
    return{
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      console.log("Lista de pokemon recebida!");
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;

    }).catch(err => {
      console.log("erro ao pegar lista de pokemon",err);
    });
  },
  components: {
    PokemonList,
  },
  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' ') {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  },
  computed: {
    /*
    resultadoBusca: function() {
      if(this.busca == '' || this.busca == ' ') {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
      */
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
}

.image-border {
    border-radius: 10px;
}
</style>
