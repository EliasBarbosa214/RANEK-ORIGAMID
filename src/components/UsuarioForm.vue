<template>
  <form action="">
    <div class="usuario" v-if="mostrarDadosLogin">
      <label for="nome">Nome:</label>
      <input type="text" id="nome" name="nome" v-model="nome" />

      <label for="email">Email:</label>
      <input type="email" id="email" name="email" v-model="email" />

      <label for="senha">Senha:</label>
      <input type="password" id="senha" name="senha" v-model="senha" />
    </div>

    <label for="cep">CEP:</label>
    <input
      type="text"
      id="cep"
      name="cep"
      v-model="cep"
      @keyup="preencherCep"
    />

    <label for="rua">Rua:</label>
    <input type="text" id="rua" name="rua" v-model="rua" />

    <label for="numero">Número:</label>
    <input type="text" id="numero" name="numero" v-model="numero" />

    <label for="bairro">Bairro:</label>
    <input type="text" id="bairro" name="bairro" v-model="bairro" />

    <label for="cidade">Cidade:</label>
    <input type="text" id="cidade" name="cidade" v-model="cidade" />

    <label for="estado">Estado:</label>
    <input type="text" id="estado" name="estado" v-model="estado" />

    <!-- {{ this.$store.state.usuario }} -->

    <div class="button">
      <slot></slot>
    </div>
  </form>
</template>

<script>
/* import { mapFields } from "@/helpers.js"; */
import { getCep } from "@/services.js";

export default {
  computed: {
    mostrarDadosLogin() {
      return !this.$store.state.login || this.$route.name === "usuario-editar";
    },
    /* ...mapFields({
      fields: [
        "nome",
        "email",
        "senha",
        "rua",
        "cep",
        "numero",
        "bairro",
        "cidade",
        "estado",
      ],
      base: "usuario",
      mutation: "UPDATE_USUARIO",
    }), */

    nome: {
      get() {
        return this.$store.state.usuario.nome;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { nome: value });
      },
    },

    email: {
      get() {
        return this.$store.state.usuario.email;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { email: value });
      },
    },

    senha: {
      get() {
        return this.$store.state.usuario.senha;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { senha: value });
      },
    },

    rua: {
      get() {
        return this.$store.state.usuario.rua;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { rua: value });
      },
    },

    cep: {
      get() {
        return this.$store.state.usuario.cep;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { cep: value });
      },
    },

    numero: {
      get() {
        return this.$store.state.usuario.numero;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { numero: value });
      },
    },

    bairro: {
      get() {
        return this.$store.state.usuario.bairro;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { bairro: value });
      },
    },

    cidade: {
      get() {
        return this.$store.state.usuario.cidade;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { cidade: value });
      },
    },

    estado: {
      get() {
        return this.$store.state.usuario.estado;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { estado: value });
      },
    },
  },

  methods: {
    preencherCep() {
      const cep = this.cep.replace(/\D/g, "");
      if (cep.length === 8) {
        getCep(cep).then((r) => {
          console.log(r);
          this.rua = r.data.logradouro;
          this.bairro = r.data.bairro;
          this.estado = r.data.uf;
          this.cidade = r.data.localidade;
        });
      }
    },
  },
};
</script>

<style scoped>
.usuario,
form {
  display: grid;
  grid-template-columns: 80px 1fr;
  align-items: center;
}

.usuario {
  grid-column: 1 / 3;
}

.button {
  grid-column: 2;
}
</style>
