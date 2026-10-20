<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';

const tutor = ref(null);

async function carregarTutor() {
    const idTutor = route.params.id;

    const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`);
    console.log('respostaTutor', respostaTutor);
    tutor.value = await respostaTutor.json();
    console.log('tutor', tutor.value);
}

onMounted(carregarTutor);
</script>

<template>
 <section v-if="tutor == null">
    <p>Carregando...</p>

 </section>
 <section v-else>
    <h1>Detalhes do tutor</h1>
    <p>Visualize os dados do tutor e cadastre pets vinculados.</p>

    <h2>Informações do Tutor</h2>

    <p>Nome do tutor: {{ tutor.nome }}</p>
    <p>ID: {{ tutor.id }}</p>
    <p>Telefone: {{ tutor.telefone }}</p>
 </section>
    
</template>