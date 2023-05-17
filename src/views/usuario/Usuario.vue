<template>
    <div>
        <section class="usuario">
            <nav class="sidnav">
                <ul>
                    <li>
                        <router-link :to="{name: 'usuario'}">Produtos</router-link>
                    </li>
                    <li>
                        <router-link :to="{name: 'compras'}">Compras</router-link>
                    </li>
                    <li>
                        <router-link :to="{name: 'vendas'}">Vendas</router-link>
                    </li>
                    <li>
                        <router-link :to="{name: 'usuario-editar'}">Editar usuário</router-link>
                    </li>
                    <li>
                        <button @click.prevent="deslogar">Deslogar</button>
                    </li>
                </ul>
            </nav>
            <transition mode="out-in">
                <router-view></router-view>
            </transition>
        </section>
    </div>
</template>

<script>
export default {
    name: "comp-usuario",
    methods: {
        deslogar() {
            this.$store.dispatch("deslogarUsuario");
            this.$router.push("/login");
        }
    }
}
</script>

<style lang="scss" scoped>
    .usuario {
        display: grid;
        grid-template-columns: minmax(140px, 200px) 1fr;
        max-width: 900px;
        margin: 40px auto;
        column-gap: 30px;
        padding: 20px;

        @media screen and (max-width: 500px) {
            grid-template-columns: 1fr;
            margin: 0 auto;
            column-gap: 20px;
        }

        .sidnav {
            a, button {
                padding: 10px;
                display: block;
                background: #f4f7fc;
                margin-bottom: 10px;
                border-radius: 4px;

                &.router-link-exact-active {
                    background: #87f;
                    color: #fff;
                    transition: all .2s;
                }

                &:hover {
                    transition: all .2s;
                    background: #87f;
                    color: #fff;
                }
            }

            button {
                border: none;
                width: 100%;
                font-size: 1rem;
                text-align: left;
                font-family: "Avenir", Helvetica, Arial, sans-serif;
                cursor: pointer;
                transition: all .2s;

                &:hover {
                    background: #87f;
                    color: #fff;
                    transition: all .2s;
                }
            }
        }
    }
</style>