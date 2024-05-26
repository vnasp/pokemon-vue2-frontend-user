<template>
  <article class="card">
    <!-- Usamos rightAnswer (booleano) para condicionar todo -->
    <div class="card__image">
      <img :src="pokemon.image" :style="{ filter: pokemon.rightAnswer ? 'none' : 'blur(5px) grayscale(100%)' }">
    </div>
    <div class="card__body">
      <input type="text" placeholder="¿Quién es?" v-model="quiz" v-if="!pokemon.rightAnswer">
      <h2 class="card__title" v-if="pokemon.rightAnswer">{{ pokemon.name }}</h2>
      <button class="btn btn--primary" v-show="!pokemon.rightAnswer" @click="handleClick">
        Descubrir
      </button>
    </div>
  </article>
</template>

<script>
export default {
  name: 'PokemonCard',
  props: {
    pokemon: Object
  },
  data() {
    return {
      quiz: this.pokemon.quiz
    };
  },
  methods: {
    // $emit para enviarlo al componente padre
    handleClick() {
      this.$emit('discover', { ...this.pokemon, quiz: this.quiz });
    }
  }
}
</script>
