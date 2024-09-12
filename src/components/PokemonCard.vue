<script>
import axios from 'axios'

export default {
  props: ['pokemon'],
  name: 'PokemonCard',
  data() {
    return {
      pokemonImage: '',
      guess: '',
      whichPokemon: false,
      alert: ''
    }
  },
  async mounted() {
    try {
      const response = await axios.get(this.pokemon.url)
      this.pokemonImage = response.data.sprites.front_default
    } catch (error) {
      console.error('Error al obtener la imagen del pokémon:', error)
    }
  },
  methods: {
    checkGuess() {
      if (this.guess === this.pokemon.name) {
        this.whichPokemon = true
        this.$emit('discovered', this.pokemon.name)
      } else {
        this.alert = 'Nombre incorrecto'
      }
    }
  }
}
</script>

<template>
  <div class="pokemon-card">
    <img :src="pokemonImage" :style="whichPokemon ? '' : 'filter: blur(5px) grayscale(100%);'" />

    <div v-if="!whichPokemon">
      <input v-model="guess" placeholder="Escribe el nombre" />
      <button @click="checkGuess">Descubrir</button>
    </div>

    <div v-else>
      <h3>{{ pokemon.name }}</h3>
    </div>

    <p v-if="alert" class="alert">{{ alert }}</p>
  </div>
</template>

<style>
.pokemon-card {
  text-align: center;
}

.alert {
  color: red;
}
</style>
