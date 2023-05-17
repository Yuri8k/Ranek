<template>
    <section>
        <h2>Adicionar Produto</h2>
        <ProdutoAdicionarVue />
        <h2>Seus Produtos</h2>
        <transition-group v-if="usuario_produtos" name="list" tag="ul">
            <li v-for="(produto) in usuario_produtos" :key="produto.id">
                <ProdutoItemVue :produto="produto">
                    <p>{{ produto.descricao }}</p>
                </ProdutoItemVue>
            </li>
        </transition-group>
    </section>
</template>

<script>
import ProdutoAdicionarVue from '@/components/ProdutoAdicionar.vue'
import ProdutoItemVue from '@/components/ProdutoItem.vue';
import { mapState, mapActions } from 'vuex';

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
        ...mapActions(["getUsuarioProdutos"])
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
</style>