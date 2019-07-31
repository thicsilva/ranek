<template>
  <div class="not-found">
    <h1>Página não encontrada</h1>
    <router-link
      class="btn"
      to="/"
    >Ir para Home</router-link>
  </div>
</template>

<script>
export default {
  name: "NotFound"
}
</script>

<style scoped>
.not-found {
  height: 50vh;
}
.not-found h1 {
  text-align: center;
}
</style>
