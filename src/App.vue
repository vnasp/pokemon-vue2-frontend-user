<template>
  <div id="app">
    <PokemonHeader />
    <main>
      <section>
        <h2 class="quiz__title">Pokémon descubiertos</h2>
        <h3 class="quiz__counter">{{ rightAnswersCount }} / {{ pokemons.length }}
        </h3>
      </section>
      <section class="cards">
        <PokemonCard v-for="(pokemon, index) in pokemons" :key="index" :pokemon="pokemon" @discover="discoverPokemon" />
      </section>
    </main>
    <PokemonFooter />
  </div>
</template>

<script>
import PokemonHeader from './components/PokemonHeader.vue'
import PokemonCard from './components/PokemonCard.vue'
import PokemonFooter from './components/PokemonFooter.vue'
import axios from 'axios';
import Swal from 'sweetalert2'

export default {
  name: 'App',
  components: {
    PokemonCard,
    PokemonHeader,
    PokemonFooter
  },
  data: () => ({
    pokemons: [],
  }),
  mounted() {
    this.fetchPokemons();
  },
  methods: {
    async fetchPokemons() {
      try {
        // Obtenemos 20 pokemons (url viene con límite)
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon');
        const pokemons = response.data.results;

        // Obtenemos la info de cada uno y los pasamos al array
        for (const pokemon of pokemons) {
          try {
            const details = await axios.get(pokemon.url);
            this.pokemons.push({
              name: pokemon.name,
              image: details.data.sprites.other['official-artwork'].front_default,
              quiz: '',
              rightAnswer: false
            });
          } catch (error) {
            console.error("Error al obtener los detalles del Pokémon: ", pokemon.name, error);
          }
        }
      } catch (error) {
        console.error("Hubo un error al obtener los Pokémon: ", error);
        alert('Hubo un error al obtener los Pokémon.');
      }
    },
    discoverPokemon(pokemon) {
      // Busca el índice del Pokémon en el array por su nombre
      const index = this.pokemons.findIndex(p => p.name === pokemon.name);
      // Si el input es igual al nombre, actualiza respuesta correcta a verdadero
      if (pokemon.quiz.toLowerCase() === pokemon.name.toLowerCase()) {
        this.pokemons[index].rightAnswer = true;
      } else {
        Swal.fire({
          title: 'Respuesta Incorrecta',
          text: "Revisa si el nombre está bien escrito",
          icon: 'error',
        });
      }
    }
  },
  computed: {
    rightAnswersCount() {
      // Filtra por respuesta correcta verdadero
      const rightAnswers = this.pokemons.filter(pokemon => pokemon.rightAnswer === true);
      // Devuelve el largo del array filtrado
      return rightAnswers.length;
    }
  }
}
</script>

<style></style>
