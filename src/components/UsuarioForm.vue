<template >
    <form>
        <div class="usuario" v-if="mostrarDadosLogin">   
            <label for="nome">Nome</label>
            <input type="text" id="nome" name="nome" v-model="nome">
            <label for="email">Email</label>
            <input type="email" id="email" name="email" v-model="email">
            <label for="senha">Senha</label>
            <input type="password" id="senha" name="senha" v-model="senha">
        </div>
        <label for="rua">Cep</label>
        <input type="text" id="cep" name="cep" v-model="cep" @keyup="preencherCep">
        <label for="rua">Rua</label>
        <input type="text" id="rua" name="rua" v-model="rua">
        <label for="numero">Número</label>
        <input type="text" id="numero" name="numero" v-model="numero">
        <label for="bairro">Bairro</label>
        <input type="text" id="bairro" name="bairro" v-model="bairro">
        <label for="cidade">Cidade</label>
        <input type="text" id="cidade" name="cidade" v-model="cidade">
        <label for="estado">Estado</label>
        <input type="text" id="estado" name="estado" v-model="estado">
        <div class="button">
            <slot></slot>
        </div>
    </form>
</template>

<script>
import { mapFields } from "@/helpers.js"
import { getCep } from "@/services.js"

export default {
    name: 'comp-usuario',
    computed: {
        ...mapFields({
            fields: ["nome", "email", "senha", "cep", "rua", "numero", "bairro", "cidade", "estado" ],
            base: "usuario",
            mutation: "UPDATE_USUARIO"
        }),
        mostrarDadosLogin() {
            return !this.$store.state.login || this.$route.name === 'usuario-editar'
        }
    },
    methods: {
        preencherCep() {
            const cep = this.cep.replace(/\D/g, "");
            if(cep.length === 8) {
                getCep(cep).then(r => {
                    this.rua = r.data.logradouro;
                    this.bairro = r.data.bairro;
                    this.estado = r.data.uf;
                    this.cidade = r.data.localidade
                })
            }
        }
    }
}   
</script>

<style lang="scss" scoped>
    form, .usuario {
        display: grid;
        grid-template-columns: 80px 1fr;
        align-items: center;
        row-gap: 10px;
    }

    .usuario {
            grid-column: 1 / 3;
    }

    label {
        margin-bottom: 0;
    }

    .button {
        grid-column: 2;
        margin-top: 10px;
    }
</style>