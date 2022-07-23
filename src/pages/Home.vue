<script setup>
import { ref } from "vue";
import Navbar from '../components/Navbar.vue';
import Banner from '../components/Banner.vue';
import EL from "../assets/EL.png";

const isDescShow = ref(false);


</script>
<template>
    <Navbar color="bg-yellow-400" @open-modal="isModalShow = true"/>
    <Banner 
    @click="isDescShow = !isDescShow"
    title="Home" 
    color="bg-blue-400" 
    :image="EL"
    desc="Semua bermula dari awal yang baik"
    :is-Show="isDescShow" 
    />
    
</template>