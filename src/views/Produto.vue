<template>
  <div v-if="produto" class="produto">
    <ul class="fotos" v-if="produto.fotos">
      <li v-for="(foto, index) in produto.fotos" :key="index">
        <img :src="foto.src" :alt="foto.titulo" />
      </li>
    </ul>
    <div class="info">
      <h1>{{ produto.nome }}</h1>
      <p class="preco">{{ numberPreco }}</p>
      <p class="descricao">{{ produto.descricao }}</p>
      <transition mode="out-in" name="fade" v-if="produto.vendido === 'false'">
        <button class="btn" v-if="!finalizar" @click="finalizar = true">
          Comprar
        </button>
        <FinalizarCompra v-else :produto="produto" />
      </transition>
      <button class="btn" v-else disabled>Produto Vendido</button>
    </div>
  </div>

  <div v-else>
    <PaginaCarregando />
  </div>
</template>

<script>
import { api } from "@/services.js";
import PaginaCarregando from "@/components/PaginaCarregando.vue";
import FinalizarCompra from "@/components/FinalizarCompra.vue";

export default {
  name: "Produtos",
  props: ["id"],
  components: {
    PaginaCarregando,
    FinalizarCompra,
  },
  data() {
    return {
      produto: null,
      numberPreco: null,
      finalizar: false,
    };
  },
  methods: {
    getProduto() {
      setTimeout(() => {
        api.get(`/produto/${this.id}`).then((r) => {
          this.produto = r.data;
          var number = Number(this.produto.preco);
          number = number.toLocaleString("pt-BR", {
            style: "currency",
            currency: "BRL",
          });
          this.numberPreco = number;
        });
      }, 1500);
    },
  },

  created() {
    this.getProduto();
  },
};
</script>

<style scoped>
.produto {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 30px;
  max-width: 900px;
  padding: 60px 20px;
  margin: 0 auto;
}

.preco {
  color: #e80;
  font-weight: bold;
  font-size: 1.5rem;
  margin-bottom: 40px;
}

.descricao {
  font-size: 1.2rem;
}

.btn {
  margin-top: 60px;
  width: 200px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
