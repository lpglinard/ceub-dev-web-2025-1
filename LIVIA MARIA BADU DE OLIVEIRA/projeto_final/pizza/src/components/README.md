# Componentes Reutilizáveis

## MensagemComponent

O `MensagemComponent` é um componente reutilizável para exibir mensagens de alerta ou sucesso na aplicação.

### Propriedades (Props)

| Prop | Tipo | Padrão | Descrição |
|------|------|--------|-----------|
| tipo | String | 'alerta' | Tipo da mensagem ('alerta' ou 'sucesso') |
| texto | String | - | Texto da mensagem (obrigatório) |
| mostrar | Boolean | false | Controla a visibilidade da mensagem |

### Eventos

| Evento | Descrição |
|--------|-----------|
| fechar | Emitido quando o botão de fechar é clicado |

### Estilos

- **Alerta**: Fundo vermelho claro com texto vermelho escuro
- **Sucesso**: Fundo verde claro com texto verde escuro

### Exemplo de Uso

```vue
<template>
  <div>
    <mensagem-component 
      :tipo="mensagem.tipo" 
      :texto="mensagem.texto" 
      :mostrar="mensagem.mostrar" 
      @fechar="fecharMensagem"
    />
    <!-- Resto do seu componente -->
  </div>
</template>

<script>
import MensagemComponent from '@/components/MensagemComponent.vue';

export default {
  components: {
    MensagemComponent
  },
  data() {
    return {
      mensagem: {
        tipo: 'alerta',
        texto: '',
        mostrar: false
      }
    };
  },
  methods: {
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
  }
}
</script>
```

### Dicas de Uso

1. Use o método `exibirMensagem` para mostrar mensagens em diferentes situações:
   - Mensagens de sucesso após uma operação bem-sucedida
   - Mensagens de alerta para erros ou avisos

2. O componente usa `v-show` para controlar a visibilidade, o que é mais eficiente para elementos que alternam visibilidade frequentemente.

3. A mensagem pode ser fechada manualmente pelo usuário ou automaticamente após um tempo definido.
