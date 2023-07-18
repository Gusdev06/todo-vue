<script setup>
import { reactive } from 'vue';



const estado = reactive({
  tarefatemp: '',
  filtro: 'todas',
  tarefas: [
    {
      titulo: 'estudar ES6',
      Finalizada: false,
    },
    {
      titulo: 'tomar remedio',
      Finalizada: false,
    },
    {
      titulo: 'ir a academia',
      Finalizada: true,
    }
  ]
}) 

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.Finalizada)

}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.Finalizada)

}

const tarefasFiltradas = () => {
  const { filtro} = estado
  
  switch(filtro) {
    case 'pendentes':
      return getTarefasPendentes()
    case 'Finalizadas':
      return getTarefasFinalizadas()
    default: 
      return estado.tarefas
  
  }
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: estado.tarefatemp,
    finalizada: false,
  }

  estado.tarefas.push(novaTarefa)
  estado.tarefatemp = ''
}


</script>

<template>
<div class="container">
  <header class="p-5 mb-4 bg-light rounded-4">
    <h1>Minhas tarefas</h1>
    <p>
      Voce possui {{ getTarefasPendentes().length }} tarefas pendentes
    </p>
  </header>
  <form  @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefatemp" @change="evento => estado.tarefatemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="evento => estado.filtro = evento.target.value " class="form-control">
          <option value="todas">Todas as tarefas</option>
          <option value="pendentes">Tarefas Pendentes</option>
          <option value="Finalizadas">Tarefas Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  <ul class="list-group mt-4">
    <li class="list-group-item " v-for="tarefa in tarefasFiltradas()">
      <input @change="evento => tarefa.Finalizada = evento.target.checked" :checked="tarefa.Finalizada" :id="tarefa.titulo" type="checkbox">
      <label  :class="{done: tarefa.Finalizada}" class="ms-3" :for="tarefa.titulo">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>
</div>
</template>

<style scoped>

.done {
  text-decoration: line-through;
}

</style>
