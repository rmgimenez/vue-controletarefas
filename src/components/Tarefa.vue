<template>
  <div class="card mb-2" style="width: 30rem">
    <div class="card-body">
      <h5 class="card-title" :class="{ 'text-decoration-line-through': tarefa.concluida }">
        {{ tarefa.descricao }}
      </h5>
      <p class="card-text">Data de criação: {{ dataCriacaoTarefa() }}</p>
      <button
        @click="tarefa.concluida = !tarefa.concluida"
        class="btn"
        :class="{
          'btn-warning': tarefa.concluida,
          'btn-success': !tarefa.concluida,
        }"
      >
        {{ tarefa.concluida ? 'Reativar' : 'Finalizar' }}
      </button>
      <button @click="excluirTarefa" class="btn btn-danger ms-2">Excluir</button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  tarefa: Object,
});

const emit = defineEmits(['onExcluirTarefa']);

function excluirTarefa() {
  // emite o evento para ser escutado pelo componente pai
  emit('onExcluirTarefa');
}

function dataCriacaoTarefa() {
  return props.tarefa.dataCriacao.toLocaleDateString('pt-BR');
}
</script>

<style scoped></style>
