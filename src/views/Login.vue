<template>
  <section class="login">
    <h1>Login</h1>
    <form autocomplete="off">
      <label for="email">Email</label>
      <input
        type="email"
        name="email"
        id="email"
        v-model="login.email"
      >
      <label for="senha">Senha</label>
      <input
        type="password"
        name="senha"
        id="senha"
        v-model="login.senha"
      >
      <button
        class="btn"
        @click.prevent="logar"
      >Logar</button>
      <ErroNotificacao :erros="erros" />
    </form>
    <p class="perdeu">
      <a
        href="http://localhost:8000/wp-login.php?action=lostpassword"
        target="_blank"
      >Perdeu a senha? Clique aqui</a>
    </p>
    <LoginCriar />
  </section>
</template>

<script>
import LoginCriar from "@/components/LoginCriar.vue";
export default {
  name: "Login",
  data () {
    return {
      login: {
        email: "",
        senha: "",
      },
      erros: [],
    }
  },
  components: {
    LoginCriar
  },
  methods: {
    async logar () {
      this.erros = [];
      try {
        await this.$store.dispatch("logarUsuario", this.login);
        await this.$store.dispatch("getUsuario");

        this.$router.push({ name: "usuario" });

      } catch (error) {
        this.erros.push(error.response.data.message);
      }
    }
  },
  created () {
    document.title = "Login"
  },
}
</script>

<style scoped>
.login {
  max-width: 500px;
  margin: 0 auto;
  padding: 0 20px;
}

form {
  display: grid;
}
.btn {
  width: 100%;
  max-width: 300px;
  margin: 0 auto;
}

.perdeu {
  text-align: center;
  margin: 20px auto 0 auto;
}

.perdeu a:hover {
  color: #87f;
  text-decoration: underline;
}

h1 {
  text-align: center;
  font-size: 2rem;
  margin-top: 40px;
  color: #87f;
}
</style>
