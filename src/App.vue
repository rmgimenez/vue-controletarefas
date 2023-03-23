<template>
  <div class="container mt-2">
    <div class="row">
      <div class="col"><h1>Lista de tarefas</h1></div>
    </div>

    <div class="row">
      <div class="col">
        <div class="input-group mb-3">
          <span class="input-group-text" id="basic-addon1">Descrição</span>
          <input
            type="text"
            class="form-control"
            aria-describedby="basic-addon1"
            ref="inputDescricao"
            v-model="descricao"
            @keyup.enter="adicionarTarefa"
          />
          <span @click="adicionarTarefa" class="input-group-text btn btn-primary">+</span>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <button @click="mostrarTarefasConcluidas = false" class="btn btn-primary">
          Esconder tarefas concluídas
        </button>

        <button @click="mostrarTarefasConcluidas = true" class="btn btn-primary ms-2">
          Mostrar todas as tarefas
        </button>
        <p v-if="mostrarTarefasConcluidas" class="fst-italic">Mostrando tarefas concluídas</p>
        <p v-else class="fst-italic">Escondendo tarefas concluídas</p>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <div v-for="tarefa in tarefas" :key="tarefa.codigo">
          <Tarefa
            v-if="
              mostrarTarefasConcluidas === true ||
              (mostrarTarefasConcluidas === false && tarefa.concluida === false)
            "
            :tarefa="tarefa"
            @onExcluirTarefa="excluirTarefa(tarefa)"
          ></Tarefa>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Tarefa from './components/Tarefa.vue';
import { ref, onMounted, watch } from 'vue';

const inputDescricao = ref(null);
const descricao = ref('');
const mostrarTarefasConcluidas = ref(true);

const tarefas = ref([]);

const adicionarTarefa = () => {
  if (descricao.value.trim() !== '') {
    // insere a tarefa no array de tarefas
    tarefas.value.push({
      codigo: tarefas.value.length + 1,
      descricao: descricao.value,
      concluida: false,
      dataCriacao: new Date(),
    });

    // limpa o input de descrição
    descricao.value = '';

    // seta o foco no input de descrição de tarefa
    inputDescricao.value.focus();
  }
};

watch(
  () => tarefas,
  (newValue, oldValue) => {
    // atribui o valor do novo array para o localstorage
    localStorage.setItem('tarefas', JSON.stringify(newValue.value));
  },
  { deep: true }
);

const excluirTarefa = (tarefa) => {
  // retorna um novo array retirando a tarefa passada no parâmetro
  tarefas.value = tarefas.value.filter((t) => t !== tarefa);
};

onMounted(() => {
  // seta o foco para o input de descrição
  inputDescricao.value.focus();

  // lê os dados do localstorage do browser e insere no array de tarefas
  const tarefasStorage = JSON.parse(localStorage.getItem('tarefas') || '[]');
  tarefasStorage.forEach((tarefaStorage) => {
    tarefas.value.push({
      codigo: tarefaStorage.codigo,
      descricao: tarefaStorage.descricao,
      concluida: tarefaStorage.concluida,
      dataCriacao: new Date(tarefaStorage.dataCriacao),
    });
  });
});
</script>

<style></style>
