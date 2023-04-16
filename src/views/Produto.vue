<template>
    <section>
        <transition mode="out-in">
            <div v-if="produto" class="produto">
                <ul class="fotos" v-if="produto.fotos">
                    <li v-for="(foto, index) in produto.fotos" :key="index">
                        <img :src="foto.src" :alt="foto.nome">
                    </li>
                </ul>
                <div class="info">
                    <h1>{{ produto.nome }}</h1>
                    <p class="preco">{{ produto.preco | numeroPreco }}</p>
                    <p class="descricao">{{ produto.descricao }}</p>
                    <button class="btn" v-if="produto.vendido === 'false'">Comprar</button>
                    <button class="btn" disabled v-else>Produto sem estoque</button>
                </div>
            </div>
            <PaginaCarregando v-else/>
        </transition>
    </section>
</template>


<script>
import { api } from "@/services.js"

export default {
    name: "comp-produto",
    props: ["id"],
    data() {
        return {
            produto: null
        }
    },
    methods: {
        getPreoduto() {
            api.get(`/produto/${this.id}`)
                .then(r => this.produto = r.data)
        }
    },
    created() {
        this.produto = null
        setTimeout(() => {
            this.getPreoduto();
        }, 1500)
    }
}
</script>


<style lang="scss" scoped>
    .produto {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 30px;
        max-width: 900px;
        padding: 60px 20px;
        margin: 0 auto;

        .info {
            .preco {
                color: #e80;
                font-weight: bold;
                font-size: 1.5rem;
                margin-block: 40px;
            }
            .descricao {
                font-size: 1.2rem;
            }
            .btn {
                margin-top: 60px;
                width: 200px;
            }
        }
    }
    .loading {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
</style>