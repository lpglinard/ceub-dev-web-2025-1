<template>
    <div>
        <h1>Configurar Pedido</h1>
        <pedido-component :pizza="this.pizzaSelecionada"/>
    </div>
</template>

<script>
    import PedidoComponent from '@/components/PedidoComponent.vue';
    export default {
        name : "ConfiguracaoPedidoView",
        components : {
           PedidoComponent
        },
        data() {
            return {
                pizzaSelecionada : null
            }
        },
        mounted(){
           const query = this.$route.query;
           if(query.pizza){
            const decodePizza = JSON.parse(decodeURIComponent(query.pizza));
            this.pizzaSelecionada = decodePizza;
           }


        }
    }
</script>

<style scoped>
</style>
