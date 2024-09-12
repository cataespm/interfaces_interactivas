<script>
import axios from 'axios'
import PokemonCard from './PokemonCard.vue'

export default {
  components: { PokemonCard },
  name: 'PokemonWho',
  data() {
    return {
      pokemones: []
    }
  },
  async mounted() {
    try {
      const response = await axios.get('https://pokeapi.co/api/v2/pokemon?=20')
      this.pokemones = response.data.results
    } catch (error) {
      console.error('Error al obtener los pokemones:', error)
    }
  },
  computed: {
    counter() {
      return this.pokemones.filter((pokemon) => pokemon.whichPokemon).length
    }
  },
  methods: {
    descubrir(nombre) {
      for (let i = 0; i < this.pokemones.length; i++) {
        let element = this.pokemones[i]
        if (element.name == nombre) {
          element.whichPokemon = true
        }
      }
    }
  }
}
</script>

<template>
  <div>
    <h1>¿Quién es ese Pokémon?</h1>
    <p>Pokemones descubiertos: {{ counter }}</p>

    <div style="display: flex; flex-flow: row wrap">
      <PokemonCard
        v-for="(pokemon, index) in pokemones"
        :key="index"
        :pokemon="pokemon"
        @discovered="descubrir"
      />
    </div>
  </div>
</template>

<style></style>
