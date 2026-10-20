<script setup>
import { onMounted, ref } from 'vue';

const tutores = ref([]);
const pets = ref([]);
const API_URL = 'http://localhost:3000';

async function carregarDados() {
 
  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();

  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  console.log('TUTORES -', tutores.value);
  console.log('PETS -', pets.value);
}

function nomeDoPet(tutorId) {
  const array = ref([]);
  for (const objeto of pets.value) {
    if (objeto.tutorId === tutorId) {
      array.value.push(objeto.nome);
    }
  }
  return array.value
}


async function excluirTutor(id) {
  const confirmarExclusao = window.confirm('Deseja realmente excluir?');

  if(confirmarExclusao){
    await fetch(`${API_URL}/tutores/${id}`,{
      method: 'DELETE',
    });
    await carregarDados();
  }
}

onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">
        Consulte os Tutores cadastrados no sistema.
      </p>
    </header>

    <table class="table table-striped table-hover">
      <thead>
        <th>ID</th>
        <th>Nome</th>
        <th>Telefone</th>
        <th>Nomes dos Pets</th>
        <th>Quantidade de Pets</th>
        <th>Ações</th>
      </thead>

      <tbody>
        <tr v-for="tutor in tutores" :key="tutor.id" >
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.telefone }}</td>
          <td>{{ nomeDoPet(tutor.id).join(' / ') }}</td>
          <td>{{ nomeDoPet(tutor.id).length }}</td>
          <td>
            <RouterLink :to="`/tutores/${tutor.id}`">Visualizar</RouterLink>
            <button class="btn btn-danger" @click="excluirTutor(tutor.id)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
