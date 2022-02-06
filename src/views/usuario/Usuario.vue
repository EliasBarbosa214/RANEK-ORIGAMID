<template>
  <section class="usuario">
    <nav class="sidenav">
      <ul>
        <li><router-link :to="{ name: 'usuario' }">Produtos</router-link></li>
        <li><router-link :to="{ name: 'compras' }">Compras</router-link></li>
        <li><router-link :to="{ name: 'vendas' }">Vendas</router-link></li>
        <li>
          <router-link :to="{ name: 'usuario-editar' }"
            >Editar Usuário</router-link
          >
        </li>
        <li><button @click.prevent="deslogar">Deslogar</button></li>
      </ul>
    </nav>

    <router-view v-slot="{ Component }" mode="out-in">
      <transition>
        <component :is="Component" />
      </transition>
    </router-view>
  </section>
</template>

<script>
export default {
  name: "Usuario",
  methods: {
    deslogar() {
      this.$store.dispatch("deslogarUsuario");
      this.$router.push("/login");
    },
  },
};
</script>

<style>
li {
  display: inline-block;
}

li a {
  padding: 2px 8px;
  border-radius: 2px;
  margin: 4px;
  transition: all 0.3s;
}

li a:hover {
  color: white;
  background: #87f;
}
</style>
