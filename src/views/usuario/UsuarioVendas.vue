<template>
  <section>
    <div v-if="vendas">
      <h2>Vendas</h2>
      <div
        class="produtos-wrapper"
        v-for="(venda,index) in vendas"
        :key="index"
      >
        <ProdutoItem
          v-if="venda.produto"
          :produto="venda.produto"
        >
          <p class="comprador">
            <span>Comprador: </span>
            {{venda.comprador_id}}
          </p>
        </ProdutoItem>
        <div class="entrega">
          <h3>Entrega</h3>
          <ul v-if="venda.endereco">
            <li
              v-for="(value,key) in venda.endereco"
              :key="key"
            >
              {{key}}: {{value}}
            </li>
          </ul>
        </div>
      </div>
    </div>
    <PaginaCarregando v-else />
  </section>
</template>

<script>
import ProdutoItem from '@/components/ProdutoItem.vue';
import { api } from '@/services.js';
import { mapState } from 'vuex';

export default {
  name: 'UsuarioCompra',
  data () {
    return {
      vendas: null
    }
  },
  components: {
    ProdutoItem
  },
  computed: {
    ...mapState(['usuario', 'login'])
  },
  methods: {
    getCompras () {
      api.get(`/transacao?tipo=vendedor_id`).then(response => {
        this.vendas = response.data;
      })
    }
  },
  created () {
    if (this.login) {

      this.getCompras();
    }
    document.title = "Usuario / Vendas"
  },
  watch: {
    login () {
      this.getCompras();
    }
  },

}
</script>

<style scoped>
.produto-wrapper {
  margin-bottom: 40px;
}
.comprador span {
  color: #e80;
}
h2 {
  margin-bottom: 20px;
}

.entrega {
  display: grid;
  grid-template-columns: minmax(100px, 200px) 1fr;
  grid-gap: 20px;
  margin-bottom: 60px;
}

h3 {
  justify-self: end;
  margin: 0;
}

@media screen and (max-width: 500px) {
  .entrega {
    grid-template-columns: 1fr;
    grid-gap: 10px;
  }
  h3 {
    justify-self: start;
    margin: 0;
  }
}
</style>

