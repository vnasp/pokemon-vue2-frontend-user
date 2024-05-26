<template>
  <footer class="footer">
    <p>© {{ currentYear }} Todos los derechos reservados</p>
    <p>
      Sitio creado con <a href="https://v2.vuejs.org" target="_blank" rel="noopener noreferrer"> Vue2</a> y <a
        href="https://tailwindcss.com/" target="_blank" rel="noopener noreferrer">TailwindCSS</a>
    </p>
  </footer>
</template>

<script>
export default {
  name: 'PokemonFooter',
  data() {
    return {
      currentYear: new Date().getFullYear()
    };
  }
}
</script>

<style></style>