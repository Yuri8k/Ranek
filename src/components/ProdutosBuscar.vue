<template>
  <form action="">
    <input name="busca" id="busca" type="text" v-model="busca" placeholder="Buscar...">
    <input type="submit" id="lupa" value="Buscar" @click.prevent="buscarProdutos">
  </form>
</template>

<script>
export default {
    name: 'ProdutosBuscar',
    data(){
        return {
            busca: ""
        }
    },
    methods: {
        buscarProdutos() {
            this.$router.push({query: {q: this.busca}});
        }
    }
}
</script>

<style scoped lang="scss">
   form {
        max-width: 600px;
        margin: 30px auto 60px;
        position: relative;

        #busca {
            width: 100%;
            padding: 20px;
            border: none;

            &:focus, &:hover {
                transform: scale(1.1);
            }
        }

        #lupa {
            width: 62px;
            height: 62px;
            background: url(../assets/search.svg) no-repeat center center;
            text-indent: -150px;
            border: none;
            cursor: pointer;
            position: absolute;
            top: 0px;
            right: 0;
            box-shadow: none;
        }
   }
</style>