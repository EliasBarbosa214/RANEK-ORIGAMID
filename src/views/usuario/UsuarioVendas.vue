<template>
  <section>
    <div v-if="vendas">
      <h2>Vendas</h2>
      <div
        class="produtos-wrapper"
        v-for="(venda, index) in vendas"
        :key="index"
      >
        <ProdutoItem v-if="venda.produto" :produto="venda.produto"
          ><p class="comprador">
            <span>Comprador:</span> {{ venda.comprador_id }}
          </p>
        </ProdutoItem>
        <div class="entrega">
          <h3>Entrega:</h3>
          <ul v-if="venda.endereco">
            <li v-for="(value, key) in venda.endereco" :key="key">
              {{ key }}: {{ value }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import ProdutoItem from "@/components/ProdutoItem.vue";
import { api } from "@/services.js";
import { mapState } from "vuex";

export default {
  data() {
    return {
      vendas: null,
    };
  },
  computed: {
    ...mapState(["login", "usuario"]),
  },
  components: {
    ProdutoItem,
  },
  methods: {
    getVendas() {
      api.get(`/transacao?comprador_id=${this.usuario.id}`).then((response) => {
        this.vendas = response.data;
      });
    },
  },

  watch: {
    login() {
      this.getVendas();
    },
  },

  created() {
    if (this.login) {
      this.getVendas();
    }
  },
};
</script>

<style scoped>
.produto-wrapper {
  margin-bottom: 40px;
}

.comprador span {
  color: #e80;
  font-weight: bold;
}

.entrega {
  display: grid;
  grid-template-columns: minmax(100px, 200px) 1fr;
  grid-gap: 20px;
  margin-bottom: 60px;
  margin-top: 30px;
}

h2 {
  margin-bottom: 20px;
}

h3 {
  margin: 0;
  justify-self: end;
}
</style>
