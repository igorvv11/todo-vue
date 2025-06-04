<script setup>
import { reactive } from 'vue';


  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefa: [

      {
        titulo: 'Estudar ES6',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
        {
        titulo: 'Ir para a academia',
        finalizada: true,
      }
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefa.filter(tarefas => !tarefas.finalizada)
  }

  const getTarefasFinalizada = () => {
    return estado.tarefa.filter(tarefas => tarefas.finalizada)
  }

  const getTarefasFiltradas = () => {
    const {filtro} = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case  'finalizadas':
        return getTarefasFinalizada();
      default:
        return estado.tarefa;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefa.push(tarefaNova)
    estado.tarefaTemp = '';
  }
</script>
<template>
<div class="container">
  <header class="p-5 mb-4 mt-4 bg-light rounded-3">
    <h1>Minhas tarefas</h1>
    <p>
      voce possui {{getTarefasPendentes().length}} tarefas pendentes
    </p>
  </header>
  <form @submit.prevent="cadastraTarefa" >
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
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
      <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{ done: tarefa.finalizada === true}" class="ms-3" :for="tarefa.titulo">
        {{tarefa.titulo}}
      </label>
    </li>
  </ul>
</div>
</template>

<style scoped>
.done{
  text-decoration: line-through;
}
</style>