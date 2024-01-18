<script setup>
import { reactive } from 'vue'
import Cabecalho from './components/Cabecalho.vue'
import Formulario from './components/Formulario.vue'
import ListaDeTarefas from './components/ListaDeTarefas.vue'

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizado: false
    },
    {
      titulo: 'Estudar SASS',
      finalizado: false
    },
    {
      titulo: 'Ir para a Academia',
      finalizado: true
    },
    {
      titulo: 'Fazer compras no supermercado',
      finalizado: false
    },
    {
      titulo: 'Pagar contas',
      finalizado: false
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizado)
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizado)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes()
    case 'finalizadas':
      return getTarefasFinalizadas()
    default:
      return estado.tarefas
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizado: false
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}

</script>

<template>
  <div class="container">
    <Cabecalho :tarefasPendentes="getTarefasPendentes().length" />
    <Formulario :cadastraTarefa="cadastraTarefa" :tarefaTemp="estado.tarefaTemp"
      :trocarFiltro="evento => estado.filtro = evento.target.value"
      
      :editaTarefaTemp="evento => estado.tarefaTemp = evento.target.value" />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>


