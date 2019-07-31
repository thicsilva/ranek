<template>
  <div class="not-found">
    <h1>Página não encontrada</h1>
    <router-link
      class="home"
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
h1 {
  margin: 20px auto;
  text-align: center;
}

.home {
  display: block;
  text-align: center;
}
</style>
