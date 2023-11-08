<script setup>
import { def } from '@vue/shared';
import { reactive } from 'vue';

const estado = reactive({
  tarefaTemp: '',
  filtro: 'todas',
  tarefa: [
    {
      titulo: 'Estudar ES6',
      finalizada: false
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false
    },
    {
      titulo: 'Treinar Muay Thai',
      finalizada: true
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefa.filter(tarefa => !tarefa.finalizada)
}

const getQtdTarefasPendentes = () => {
  return getTarefasPendentes().length
}

const getTarefasFinalizadas = () => {
  return estado.tarefa.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtro, tarefa } = estado

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes()
    case 'finalizadas':
      return getTarefasFinalizadas()
    default:
      return tarefa
  }
}

const cadastarTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false
  }
  estado.tarefa.push(tarefaNova)
  estado.tarefaTemp = ''
}

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getQtdTarefasPendentes() }} tarefas pendentes
      </p>
    </header>
  </div>
  <form @submit.prevent="cadastarTarefa">
    <div class="row">
      <div class="col">
        <input required :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" type="text"
          placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <!-- Adicionando o evento @change para alterar o estado do filtro -->
        <select @change="evento => estado.filtro = evento.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <!-- Adicionando o evento @change para alterar a situação da tarefa -->
      <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada"
        :id="tarefa.titulo" type="checkbox">
      <label :for="tarefa.titulo" :class="{ done: tarefa.finalizada }" class="ms-3">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>
</template>

<style scoped>
.done {
  text-decoration: line-through
}
</style>
