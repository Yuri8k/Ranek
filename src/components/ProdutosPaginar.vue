<template>
    <ul v-if="paginasTotal > 1">
        <li v-for="(pagina, index) in paginas"  :key="index">
            <router-link :to="{query: query(pagina)}">{{ pagina }}</router-link>
        </li>
    </ul>
</template>
  
  <script>
  export default {
    name: 'ProdutosPaginar',
    props: {
        produtosPorPagina: {
            type: Number,
            default: 1,
        },
        produtosTotal: {
            type: Number,
            default: 1,
        }
    },
    methods: {
        query(pagina) {
            return {
                ...this.$route.query,
                _page: pagina
            }
        }
    },
    computed: {
        paginas() {
            const current = Number(this.$route.query._page);
            const range = 9;
            const offset = Math.ceil(range / 2);
            const total = this.paginasTotal;
            const pagesArray = []

            for(let i = 1; i <= total; i++) {
                pagesArray.push(i);
            }

            pagesArray.splice(0, current - offset)
            pagesArray.splice(range, total);
            
            return  pagesArray
        },
        paginasTotal() {
            const total = this.produtosTotal / this.produtosPorPagina
            return (total !== Infinity) ? Math.ceil(total) : 0;
        }
    },
  }
  </script>

<style lang="scss" scoped>

ul {
    width: 100%;
    display: flex;
    align-content: center;
    justify-content: center;
    
    li {
    display: inline-block;

    a {
        padding: 2px 8px;
        border-radius: 2px;
        margin: 4px;

        &.router-link-exact-active, &:hover {
           background: #87f;
           color: #fff; 
        }
    }
  }
}

</style>