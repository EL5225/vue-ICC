<script setup>
import { ref } from "vue";
import Navbar from '../components/Navbar.vue';
import Banner from '../components/Banner.vue';
import GE from "../assets/GE.png";

const isDescShow = ref(false);

</script>
<template>
    <Navbar color="bg-red-400" @open-modal="isModalShow = true"/>
    <Banner 
    @click="isDescShow = !isDescShow"
    title ="Contact" 
    color="bg-yellow-400" 
    :image="GE"
     desc="Semua yang berakhir pasti sudah berakhir"
    :is-Show="isDescShow"/>
</template>