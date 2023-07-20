<template>
    <section>
        <h2>Adicionar Produto</h2>
        <ProdutoAdicionarVue />
        <h2>Seus Produtos</h2>
        <transition-group v-if="usuario_produtos" name="list" tag="ul">
            <li v-for="(produto) in usuario_produtos" :key="produto.id">
                <ProdutoItemVue :produto="produto">
                    <p>{{ produto.descricao }}</p>
                    <button class="deleter" @click="deletarProduto(produto.id)">Deletar</button>
                </ProdutoItemVue>
            </li>
        </transition-group>
    </section>
</template>

<script>
import ProdutoAdicionarVue from '@/components/ProdutoAdicionar.vue'
import ProdutoItemVue from '@/components/ProdutoItem.vue';
import { mapState, mapActions } from 'vuex';
import {api} from '@/services.js'

export default {
    name: "comp-usuario-produto",
    components: {
        ProdutoAdicionarVue,
        ProdutoItemVue
    },
    computed: {
        ...mapState(["login", "usuario", "usuario_produtos"])
    },
    methods: {
        ...mapActions(["getUsuarioProdutos"]),
        deletarProduto(id) {
            const confirmar = window.confirm("Deseja remover este produto?");
            if(confirmar) {
                api.delete(`/produto/${id}`).then(() => {
                    this.getUsuarioProdutos();
                }).catch(error => {
                    console.log(error)
                })
            }
        }
    },
    watch: {
        login() {
            this.getUsuarioProdutos();
        }
    },
    created() {
        if(this.login) {
            this.getUsuarioProdutos();
        }
    }
}
</script>

<style lang="scss" scoped>
    h2 {
        margin-bottom: 20px;
    }

    .list-enter,
    .list-leave-to {
        opacity: 0;
        transform: translate3d(20px, 0, 0);
    }

    .list-enter-active,
    .list-leave-active {
        transition: all .3s;
    }

    .deleter {
        position: absolute;
        top: 0;
        right: 0;
        background: url("../../assets/remove.svg") no-repeat center;
        width: 24px;
        height: 24px;
        text-indent: -140px;
        overflow: hidden;
        cursor: pointer;
        border: none;
    }
</style>