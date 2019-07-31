<template>
  <form class="adicionar-produto">
    <label for="nome">Nome</label>
    <input
      type="text"
      name="nome"
      id="nome"
      v-model="produto.nome"
    >
    <label for="preco">Preço</label>
    <input
      type="text"
      name="number"
      id="preco"
      v-model="produto.preco"
    >
    <label for="fotos">Fotos</label>
    <input
      type="file"
      name="fotos"
      id="fotos"
      ref="fotos"
      multiple
    >
    <label for="descricao">Descrição</label>
    <textarea
      name="descricao"
      id="descricao"
      v-model="produto.descricao"
    ></textarea>
    <button
      type="submit"
      class="btn"
      @click.prevent="adicionarProduto"
    >Adicionar Produto</button>
  </form>
</template>

<script>
import { api } from '@/services.js';
export default {
  name: "ProdutoAdicionar",
  data () {
    return {
      produto: {
        nome: "",
        preco: "",
        descricao: "",
        fotos: null,
        usuario_id: "",
        vendido: "false"
      }
    }
  },
  methods: {
    formatarProduto () {
      const form = new FormData();

      const files = this.$refs.fotos.files;

      for (let i = 0; i < files.length; i++) {
        form.append(files[i].name, files[i]);
      }

      form.append("nome", this.produto.nome);
      form.append("preco", this.produto.preco);
      form.append("descricao", this.produto.descricao);
      form.append("vendido", this.produto.vendido);
      form.append("usuario_id", this.produto.usuario_id);

      return form;

    },
    async adicionarProduto (event) {
      const produto = this.formatarProduto();
      const button = event.currentTarget;

      button.innerHTML = 'Adicionando...';
      button.setAttribute('disabled', true);
      await api.post("/produto", produto);
      await this.$store.dispatch("getUsuarioProdutos");
      button.innerHTML = 'Adicionar Produto';
      button.removeAttribute('disabled')
    },

  },
}
</script>

<style scoped>
.adicionar-produto {
  display: grid;
  grid-template-columns: 100px 1fr;
  align-items: center;
  margin-bottom: 60px;
}
.btn {
  grid-column: 2;
}
</style>
