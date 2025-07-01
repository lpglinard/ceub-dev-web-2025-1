<template>
    <div>
        <mensagem-component
            :tipo="mensagem.tipo"
            :texto="mensagem.texto"
            :mostrar="mensagem.mostrar"
            @fechar="fecharMensagem"
        />
        <form id="pedido-form" @submit="criarPedido($event)">
            <div>
                <p id="nome-pizza-content">
                    {{ pizza && pizza.nome ? pizza.nome : "-" }}
                </p>
                <img id="foto-content" :src="pizza && pizza.foto ? pizza.foto : ''"/>
            </div>
            <div class="inputs" id="form-pedido">
                <label for="nome_cliente">Nome</label>
                <input
                    type="text"
                    id="nome-cliente"
                    name="nome-cliente"
                    v-model="nomeCliente"
                    placeholder="Digite seu Nome">
            </div>
             <div class="inputs">
                <label for="tipo-massa">Tipo de Massa</label>

                <select
                    name="tipo-massa"
                    id="tipo-massa"
                    v-model="tipoMassaSelecionado">
                    <option value="" selected>Selecione o tipo de massa</option>
                    <option v-for="tipoMassa in listaTiposMassa"
                            :key="tipoMassa.id"
                            :value="tipoMassa">{{ tipoMassa.descricao }}</option>
                </select>
            </div>
            <div id="opcionais-titulo" class="inputs">
                <label id="opcionais-titulo" for="Opcionais">Selecione os opcionais</label>
                <label id="opcionais-subtitulo" for="Complemento">Adicione um complemento</label>

                <div class="checkbox-container"
                     v-for="complemento in listaComplementos"
                     :key="complemento.id">
                    <input type="checkbox"
                           :name="complemento.nome"
                           :value="complemento"
                           v-model="listaComplementosSelecionados">
                    <span>{{ complemento.nome }}</span>
                </div>

                <label for="Complemento">Adcione uma bebida</label>

                 <div class="checkbox-container"
                      v-for="bebida in listaBebidas" :key="bebida.id">
                    <input type="checkbox"
                           :name="bebida.nome"
                           :value="bebida"
                           v-model="listaBebidasSelecionadas">
                    <span>{{ bebida.nome }}</span>
                </div>
            </div>
            <div class="inputs">
                <input type="submit" class="submit-btn" value="Confirmar Pedido">
            </div>
        </form>
    </div>
</template>

<script>
    import MensagemComponent from '@/components/MensagemComponent.vue';

    export default {
        name : "PedidoComponent",
        components: {
            MensagemComponent
        },
        data(){
            return {
                nomeCliente : "",
                tipoMassaSelecionado: "",
                listaComplementosSelecionados: [],
                listaBebidasSelecionadas: [],
                listaTiposMassa : [],
                listaComplementos : [],
                listaBebidas : [],
                mensagem: {
                    tipo: 'alerta',
                    texto: '',
                    mostrar: false
                }
            }
        },
        props: {
            pizza : null
        },
        methods: {
            async getTiposMassa() {
                const response = await fetch("http://localhost:3000/tipos_pontos");
                const data = await response.json();
                this.listaTiposMassa = data;
            },
             async getOpcionais() {
                const response = await fetch("http://localhost:3000/opcionais");
                const data = await response.json();
                this.listaComplementos = data.complemento;
                this.listaBebidas = data.bebidas;
            },
            async criarPedido(e) {
                e.preventDefault();

                // Validação dos campos obrigatórios
                if (!this.nomeCliente || this.nomeCliente.trim() === '') {
                    this.exibirMensagem('alerta', 'Por favor, preencha o campo Nome.');
                    return;
                }

                if (!this.tipoMassaSelecionado || this.tipoMassaSelecionado === '') {
                    this.exibirMensagem('alerta', 'Por favor, selecione o tipo de massa.');
                    return;
                }

                const dadosPedido = {
                    nome : this.nomeCliente,
                    ponto : this.tipoMassaSelecionado,
                    bebidas : Array.from(this.listaBebidasSelecionadas),
                    complemento : Array.from(this.listaComplementosSelecionados),
                    statusId: 5,
                    pizza: this.pizza
                }

                try {
                    const dadosJson = JSON.stringify(dadosPedido);
                    const req = await fetch("http://localhost:3000/pedidos", {
                        method: "POST",
                        headers: {"Content-Type" : "application/json"},
                        body: dadosJson
                    });

                    if (req.ok) {
                        this.exibirMensagem('sucesso', 'Pedido realizado com sucesso!');
                        // Redirecionar para a página de pedidos após alguns segundos
                        setTimeout(() => {
                            this.$router.push('/pedidos');
                        }, 2000);
                    }
                } catch (error) {
                    this.exibirMensagem('alerta', 'Erro ao realizar o pedido. Tente novamente.');
                }
            },

            exibirMensagem(tipo, texto) {
                this.mensagem = {
                    tipo,
                    texto,
                    mostrar: true
                };

                // Opcionalmente, esconder a mensagem após alguns segundos
                setTimeout(() => {
                    this.fecharMensagem();
                }, 5000);
            },

            fecharMensagem() {
                this.mensagem.mostrar = false;
            }

        },
        mounted(){
            this.getTiposMassa();
            this.getOpcionais();
        }

    }
</script>

<style scoped>

#foto-content {
    margin-bottom: 16px;
    border-radius: 16px;
    position: relative;
    z-index: -1;
    justify-content: center;
    width: 100%;
    height: 180px;
    object-fit: cover;
}

#nome-pizza-content {
    font-size: 43px;
    font-weight: bold;
    text-align: start;
    margin-bottom: -90px;
    margin-left: 40px;
    color: antiquewhite;
    padding: 16px;
}

#pedido-form {
    max-width: 750px;
    margin: 0 auto;
}

.inputs{
    display: flex;
    flex-direction: column;
    margin-bottom: 16px;
}

label {
    font-weight: bold;
    margin-bottom: 16px;
    color: #222;
    padding: 5px 12px;
    flex-direction: start;
    display: flex;
    border-left: 4px solid darkgoldenrod;
}

input, select {
    padding: 12px;
    width: 300px;
    border: solid #222 1px;
    border-radius: 8px;
    height: 20px;
    font-size: 12px;
}

select {
    height: 50px;
}

#opcionais-titulo {
    width: 100%;
}

#opcionais-subtitulo {
    display: flex;
    align-items: flex-start;
    align-content: center;
    width: 100%;
    margin-bottom: 12px;
}

.checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
}

.checkbox-container span,
.checkbox-container input {
    width: auto;
    height: 20px;
}

.submit-btn {
    background-color: #222;
    color: darkgoldenrod;
    font-weight: bold;
    border: solid 1px darkgoldenrod;
    border-radius: 8px;
    cursor: pointer;
    padding: 12px;
    margin: 0 auto;
    font-size: 16px;
    width: 100%;
    height: auto;
    transition: 0.5s;
}
.submit-btn:hover {
    background-color: darkgoldenrod;
    color: #222;
}


</style>
