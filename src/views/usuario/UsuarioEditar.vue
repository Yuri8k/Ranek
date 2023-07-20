<template>
    <section>
        <UsuarioForm>
            <button class="btn" @click.prevent="AtualizarUsuario">Atualizar Usuário</button>
        </UsuarioForm>
    </section>
</template>

<script>
import UsuarioForm from '@/components/UsuarioForm.vue';
import {api} from "@/services.js";

    export default {
        name: "comp-usuario-editar",
        components: {
            UsuarioForm
        },
        methods: {
            AtualizarUsuario() {
                api.put(`/usuario/${this.$store.state.usuario.id}`, this.$store.state.usuario).then(() => {
                    this.$store.dispatch("getUsuario");
                    this.$router.push({name: "usuario"});
                }).catch(error => {
                    console.log(error.response);
                });
            }
        }
    };
</script>

<style scoped>
    
</style>