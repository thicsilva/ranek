<template>
  <section>
    <h2>Adicionar Produto</h2>
    <ProdutoAdicionar />
    <h2>Seus Produtos</h2>
    <transition-group
      v-if="usuario_produtos"
      name="list"
      tag="ul"
    >
      <li
        v-for="(produto,index) in usuario_produtos"
        :key="index"
      >
        <ProdutoItem :produto="produto">
          <p>{{produto.descricao}}</p>
          <button
            class="excluir"
            @click="excluirProduto(produto.id)"
          ></button>
        </ProdutoItem>
      </li>

    </transition-group>
  </section>
</template>

<script>
import { api } from '@/services.js';
import ProdutoAdicionar from '@/components/ProdutoAdicionar.vue';
import ProdutoItem from '@/components/ProdutoItem.vue';
import { mapState, mapActions } from 'vuex';
export default {
  name: 'UsuarioProdutos',
  components: {
    ProdutoAdicionar,
    ProdutoItem
  },
  computed: {
    ...mapState(['login', 'usuario', 'usuario_produtos'])
  },
  methods: {
    ...mapActions(['getUsuarioProdutos']),
    excluirProduto (id) {
      const confirmar = window.confirm('Deseja remover este produto?');
      if (confirmar) {
        api.delete(`/produto/${id}`).then(() => {
          this.getUsuarioProdutos();
        })

      }
    }
  },
  watch: {
    login () {
      this.getUsuarioProdutos();
    }
  },
  created () {
    if (this.login) {
      this.getUsuarioProdutos()
    }
    document.title = "Usuario"
  }
}
</script>

<style scoped>
h2 {
  margin-bottom: 20px;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.3s;
}

.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translate3d(20px, 0, 0);
}

.excluir {
  position: absolute;
  top: 0;
  right: 0;
  background: url("../../assets/remove.svg") no-repeat center center;
  height: 24px;
  width: 24px;
  text-indent: -140px;
  overflow: hidden;
  cursor: pointer;
  border: none;
}
</style>
