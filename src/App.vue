<template>
  <div id="app">
    
    <img class="poke-logo" src="./assets/pokemon-logo.png" alt="">
    <h1>¿Quién es ese Pokemon?</h1>
    <h2>Pokemones descubiertos: {{ cantidadDescubiertos }}</h2>
    <!--<h2>Cantidad de aciertos; {{ cantidadAciertos }}</h2>-->

    <div class="poke">

      <div v-for="(pokemon,i) in pokemons" :key="i" >

        <PokeCard :poke="pokemon"
                  :indice="i"
                  @validarPokemon="validarPokemon" />

      </div>

    </div>
    

  </div>
</template>

<script>
import axios from 'axios'
import PokeCard from './components/PokeCard.vue'

export default {
  name: 'App',
  components:{
    PokeCard
  },

  data(){
    return{
      pokemons:[],
    }
  },

  methods:{
    async obtenerListaPokemons(){
      const url = "https://pokeapi.co/api/v2/pokemon";
      const {data} = await axios.get(url);
      const lista = data.results;
      for (const poke of lista){
        this.obtenerPokemon(poke.url);
      }
    },

    async obtenerPokemon(url){
      const {data} = await axios.get(url);
      const name = data.name;
      const img = data.sprites.other.dream_world.front_default;
      const descubierto = false;
      this.pokemons.push({name, img, descubierto});
      console.log(this.pokemons.length)
    },

    validarPokemon({nombre,indice}){

      if(this.pokemons[indice].name == nombre)
      this.pokemons[indice].descubierto = true;
        else
        alert("Nombre incorrecto :")
    }

  },

  mounted(){
    this.obtenerListaPokemons();
  },

  computed:{
    cantidadDescubiertos(){
      let cantidad = 0;
      for(let i = 0 ; i< this.pokemons.length;i++){
        if(this.pokemons[i].descubierto == true)
          cantidad++;
      }
      return cantidad;
    },

    cantidadAciertos(){
      const pokeAciertos = this.pokemons.filter((poke) => poke.descubierto == true)
        return pokeAciertos.length;
    }

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
  padding-left: 100px;
  padding-right: 100px;
}

.poke-logo{
  width: 300px;
  height: 180px;
}

.poke {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 50px;
}

</style>
