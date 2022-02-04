<template>
  <div>
    <h4>
      ◀ <span>Página: {{ $route.query._page }}</span> ▶
    </h4>

    <div>
      <ul v-if="paginasTotal > 1">
        <router-link :to="{ query: query(1) }" class="ultimo"> ◀◀ </router-link>
        <li v-for="pagina in paginas" :key="pagina">
          <router-link :to="{ query: query(pagina) }">{{ pagina }}</router-link>
        </li>
        <router-link :to="{ query: query(paginasTotal) }" class="ultimo">
          ▶▶
        </router-link>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    produtosPorPagina: {
      type: Number,
      default: 1,
    },
    produtosTotal: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    query(pagina) {
      return {
        ...this.$route.query,
        _page: pagina,
      };
    },
    queryProxima(pagina) {
      return {
        ...this.$route.query,
        _page: pagina,
      };
    },
  },
  computed: {
    paginasTotal() {
      const total = this.produtosTotal / this.produtosPorPagina;
      return total !== Infinity ? Math.ceil(total) : 0;
    },

    paginas() {
      const current = Number(this.$route.query._page);
      const range = 9;
      const offset = Math.ceil(range / 2);
      const total = this.paginasTotal;
      const pagesArray = [];

      for (let i = 1; i <= total; i++) {
        pagesArray.push(i);
      }

      pagesArray.splice(0, current - offset);
      pagesArray.splice(range, total);
      return pagesArray;
    },
  },
};
</script>

<style scoped>
div {
  grid-column: 1 / -1;
}

ul {
  text-align: center;
  margin: 10px;
  padding: 10px;
}

h4 {
  text-align: center;
  color: #87f;
}

span {
  padding: 3px 10px;
}

li {
  display: inline-block;
}

li a {
  padding: 2px 8px;
  border-radius: 2px;
  margin: 4px;
  transition: all 0.3s;
}

/* 
Não está dando certo, pq por algum motivo todas os router-link já estão com essa classes.
Problema com o JavaScript do curso da Origamid (Quase certeza que é por causa que estou usando vue 3.)

li a.router-link-exact-active,

*/

li a:hover {
  background: #87f;
  color: #fff;
}

.ultimo {
  color: #87f;
  transition: all 0.3s;
}
.ultimo:hover {
  font-size: 1rem;
  color: orange;
}

.ultimo:active {
  color: #234;
}
</style>
