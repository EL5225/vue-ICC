<script setup>
import { ref } from "vue";
import Navbar from '../components/Navbar.vue';
import Banner from '../components/Banner.vue';
import ER from "../assets/ER.png";

const isDescShow = ref(false);

</script>
<template>
    <Navbar color="bg-blue-400" @open-modal="isModalShow = true"/>
    <Banner 
    @click="isDescShow = !isDescShow"
    title="About" 
    color="bg-green-300" 
    title-color="text-white"
    :image="ER"
    desc="Semua yang berlalu pasti sudah berlalu. Jangan selalu mengingat masa lalu"
    :is-Show="isDescShow"/>
</template>