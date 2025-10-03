<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas:[
    {
      titulo: 'Lavar a louça',
      finalizada: true
    },
    {
      titulo: 'Arrumar celular',
      finzalizada: false
    },
    {
      titulo: 'Colocar prateleira',
      finalizada: false
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}
const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}
const getTarefasFiltradas = () => {
  const {filtro} = estado;
  switch(filtro){
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}
</script>

<template>
<div class="container">
  <header class="p-5 mb-4 mt-4 bg-light rounded-3 ">
    <h1>Minhas tarefas </h1>
    <p>
      Voce possui {{getTarefasPendentes().length}} Tarefas pendentes
    </p>
  </header>
  <form @submit.prevent="cadastraTarefa"> 
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Qual sua tarefa?" class="form-control" >
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary" type="submit">Cadastrar</button>
      </div>
      <div class=" col-md-2">
        <select @change="evento => estado.filtro =  evento.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Tarefas pendentes</option>
          <option value="finalizadas">Tarefas finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">{{tarefa.titulo}}</label>
    </li>
  </ul>
</div>
</template>

<style scoped>
.done{
  text-decoration: line-through
}
</style>
