<template>
  <form>
    <input
      type="text"
      name="busca"
      id="busca"
      v-model="busca"
    >
    <button
      type="submit"
      id="lupa"
      @click.prevent="buscarProdutos"
    >Buscar</button>

  </form>
</template>

<script>
export default {
  name: "ProdutosBuscar",
  data () {
    return {
      busca: ""
    }
  },
  methods: {
    buscarProdutos () {
      this.$router.push({ query: { q: this.busca } });
    }
  },
}
</script>

<style>
</style>
