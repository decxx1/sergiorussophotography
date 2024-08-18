<script setup>
import { onMounted, ref } from 'vue';

const path = ref('');
onMounted(() => {   
    path.value = window.location.pathname;
});

</script>
<template>
    <ul>
        <li :class="{'active': path === '/'}"><a href="./">Inicio</a></li>
        <li :class="{'active': path === '/galeria'}"><a href="./galeria">Galería</a></li>
        <li :class="{'active': path === '/about'}"><a href="./about">Sergio Russo</a></li>
        <li :class="{'active': path === '/contacto'}"><a href="./contacto">Contacto</a></li>
    </ul>
</template>