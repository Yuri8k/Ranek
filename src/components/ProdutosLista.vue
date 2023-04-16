<template>
    <section class="produtos-container">
        <transition mode="out-in">
            <ul v-if="produtos && produtos.length" class="produtos" key="produtos">
                <li class="produto" v-for="(produto, index) in produtos" :key="index">
                    <router-link :to="{name: 'produto', params: {id: produto.id}}">
                        <img v-if="produto.fotos" :src="produto.fotos[0].src" :alt="produto.titulo" />
                        <p class="preco">{{ produto.preco | numeroPreco }}</p>
                        <h2 class="titulo">{{ produto.nome }}</h2>
                        <p>{{ produto.descricao }}</p>
                    </router-link>
                </li>
                <ProdutosPaginar :produtosTotal="produtosTotal" :produtosPorPagina="produtosPorPagina" />
            </ul>
            <div class="sem-resultados" v-else-if="produtos && produtos.length === 0" key="sem-resultado">
                <p>Busca sem resultado. Tente buscar outro termo.</p>
            </div>
            <PaginaCarregando v-else key="carregando" />
        </transition>
    </section>
</template>

<script>
import ProdutosPaginar from "./ProdutosPaginar.vue";
import { api } from "@/services.js";
import { serialize } from "@/helpers.js";

export default {
    name: "ProdutosLista",
    components: {
        ProdutosPaginar,
    },
    data() {
        return {
            produtos: null,
            produtosPorPagina: 9,
            produtosTotal: 0,
        };
    },
    computed: {
        url() {
            const query = serialize(this.$route.query);
            return `/produto?_limit=${this.produtosPorPagina}${query}`;
        },
    },
    methods: {
        getProdutos() {
            this.produtos = null;
            setTimeout(() => {
                api.get(this.url).then((r) => {
                    this.produtosTotal = Number(r.headers["x-total-count"]);
                    this.produtos = r.data;
                });
            }, 1500);
        },
    },
    watch: {
        url() {
            this.getProdutos();
        },
    },
    created() {
        this.getProdutos();
    },
};
</script>

<style scoped lang="scss">
.produtos-container {
    max-width: 1000px;
    margin: 0 auto;

    .produtos {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 30px;
        margin: 30px;

        .produto {
            box-shadow: 0 4px 8px rgba(30, 60, 90, 0.1);
            padding: 10px;
            background: #fff;
            border-radius: 4px;
            transition: all 0.2s;

            &:hover {
                box-shadow: 0 6px 12px rgba(30, 60, 90, 0.2);
                transform: scale(1.1);
                position: relative;
                z-index: 1;
            }

            img {
                border-radius: 4px;
                margin-block: 20px;
            }

            .titulo {
                margin-block: 10px;
            }

            .preco {
                color: #e80;
                font-weight: bold;
            }
        }
    }

    .sem-resultados {
        text-align: center;
    }
}
</style>