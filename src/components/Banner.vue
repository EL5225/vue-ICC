<script setup>
defineProps({
    title:"",
    color:"",
    image:"",
    textcolor:"",
    desc:"",
    isShow: false,

})
</script>
<template>
    <main :class="color" class="flex bg-blue-400 h-screen w-full justify-center p-10">
        <div class="flex items-center justify-between w-full">
            <div class="flex flex-col">
                <h1 class="text-white font-bold text-5xl">
                    {{title}}</h1>
                <p v-if="isShow" class="max-w-sm text-white text-3xl"> 
                    {{desc}} </p>
            </div>
        </div>
        <img width="500" :src="image" alt=""/>
    </main>
</template>