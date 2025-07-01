<template>
  <div class="mensagem-container" v-show="mostrar" :class="tipo">
    <span class="mensagem-texto">{{ texto }}</span>
    <button class="fechar-btn" @click="fechar">×</button>
  </div>
</template>

<script>
export default {
  name: "MensagemComponent",
  props: {
    tipo: {
      type: String,
      default: "alerta",
      validator: (value) => ["alerta", "sucesso"].includes(value)
    },
    texto: {
      type: String,
      required: true
    },
    mostrar: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    fechar() {
      this.$emit("fechar");
    }
  }
}
</script>

<style scoped>
.mensagem-container {
  padding: 15px;
  border-radius: 5px;
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.alerta {
  background-color: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

.sucesso {
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.mensagem-texto {
  flex-grow: 1;
}

.fechar-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  padding: 0 5px;
  color: inherit;
}
</style>
