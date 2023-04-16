<template>
   <section class="login">
    <h1 class="title">Login</h1>
    <form action="" class="form">
       <label for="email">Email</label>
       <input type="email" name="email" id="email" v-model="login.email">
       <label for="senha">Senha</label>
       <input type="password" name="senha" id="senha" v-model="login.senha">
       <button @click.prevent="logar" class="btn">Logar</button>
    </form>
    <p class="perdeu">
        <a href="/" target="_blank">Perdeu a senha? Clique aqui.</a>
    </p>
    <LoginCriarVue />
   </section>
</template>

<script>
import LoginCriarVue from '@/components/LoginCriar.vue'

export default {
    name: "comp-login",
    data() {
        return {
            login: {
                email: "",
                senha: ""
            }
        }
    },
    components: {
        LoginCriarVue 
    },
    methods: {
        logar() {
            this.$store.dispatch("getUsuario", this.login.email);
            this.$router.push({ name: "usuario" });
        }
    }
}
</script>

<style lang="scss" scoped>
    .login {
        max-width: 500px;
        margin: 0 auto;
        padding: 0 20px;

        .title {
            text-align: center;
            font-size: 2rem;
            margin-top: 40px;
            color: #87f;
        }

        form {
           display: grid; 

           .btn {
            width: 100%;
            max-width: 300px;
            margin-left: auto;
            margin-right: auto;
           }
        }

        .perdeu {
            text-align: center;
            margin: 20px auto 0;

            a:hover {
                color: #87f;
                text-decoration: underline;
            }
        }
    }
</style>