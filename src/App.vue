<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/logo_guia.png" />
      <hr />
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text" name id placeholder="Buscar..." v-model="busca" class="input is-rounded" />
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>
      <div v-for="(poke, index) in filterdPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filterdPokemons: [],
      busca: ''
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("LISTA DE POKEMONS");
        this.pokemons = res.data.results;
        this.filterdPokemons = res.data.results;
      })
      .catch((err) => {
        console.log(err);
      });
  },
  methods: {
    buscar: function(){
      this.filterdPokemons = this.pokemons;
      if(this.busca.trim() == ''){
        this.filterdPokemons = this.pokemons;
      } else {
        this.filterdPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  },
  components: {
    Pokemon,
  },
  computed: {
    /*resultadoBusca: function(){
      if(this.busca.trim() == ''){
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }*/
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

#buscaBtn {
  margin-top: 2%;
}
</style>
