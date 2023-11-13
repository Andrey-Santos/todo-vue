<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

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

const cadastraTarefa = () => {
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
    <Cabecalho :tarefas-pendentes="getQtdTarefasPendentes()" />
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp"
      :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>