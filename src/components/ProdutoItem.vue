<template>
    <div class="produto" v-if="produto">
        <router-link class="produto-img" :to="{name: 'produto', params: {id: produto.id}}">
            <img v-if="produto.fotos" :src="produto.fotos[0].src" :alt="produto.fotos[0].titulo">
            <p>Ver Produto</p>
        </router-link>
       <div class="info">
            <p class="preco">{{ produto.preco | numeroPreco }}</p>
            <h2 class="titulo">{{ produto.nome }}</h2>
            <slot></slot>
       </div>
    </div>
</template>

<script>
export default {
    name: "comp-produto-item",
    props: ["produto"]
}
</script>

<style lang="scss" scoped>
    .produto {
        display: grid;
        grid-template-columns: minmax(100px, 200px) 1fr;
        column-gap: 20px;
        margin-bottom: 40px;
        position: relative;

        .produto-img {
            border-radius: 4px;
            overflow: hidden;
            height: 100px;
        }

        .info {
            align-self:end;
        }
    }
</style>