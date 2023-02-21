<template lang="">
  <h1 v-if="!pokemon">Espere por favor...</h1>
  
  <div v-else>

    <h1>¿Quién es este pokémon?</h1>
  
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer"/>
    
    <div v-if="showAnswer">
      <h2  class="fade-in">{{message}}</h2>
      <button @click="newGame" >
        Nuevo juego
      </button>
    </div>
  </div>
</template>

<script>

import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue';

import getPokemonOptions from '@/helpers/getPokemonOptions'
// getPokemonOptions()

export default {
  name: 'PokemonPage',
  components: {
    PokemonOptions,
    PokemonPicture
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: ''
    }
  }, 
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions()
      const rndInt = Math.floor(Math.random() * 4)
      this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer(selectedId) {
      if (selectedId === this.pokemon.id) {
        this.showPokemon = true
        this.message = `Correcto, es ${this.pokemon.name}`
      } else {
        this.message = `Oops... era ${this.pokemon.name}`
      }
      this.showAnswer = true;
    },
    newGame() {
      this.showPokemon = false;
      this.showAnswer = false;
      this.message = '',
      this.pokemon = null;
      this.mixPokemonArray();
    }
  },
  mounted() {
    this.mixPokemonArray()
  }
}

</script>