<script setup>
import { ref } from 'vue';
import Modal from "./Modal.vue";
import { database } from '../server'
import { compileScript } from '@vue/compiler-sfc';
const emits = defineEmits(['openModal'])
const isModalShow = ref(false);
const isLogin = ref(false);

const isAuthenticated = ref(false);
const currentFullname = ref("");

const submitLogin = () => {
    currentFullname.value = database.value.filter((x) => x.email === email.value).map((x) => x.fullname)[0]
    isModalShow.value = false;
    isAuthenticated.value = true;
    email.value = "";
    password.value = "";
}

const submitRegister = () => {
    const payload = {
        id: new Date(),
        email: email.value,
        password: password.value,
        fullname: fullname.value,
    };
    database.value.push(payload)
    isModalShow.value = false;
    email.value = "";
    password.value = "";
};

const isLogout = () => {
    currentFullname.value = "";
    isAuthenticated.value = false;
}

const openModalLogin = () => {
    emits('openModal')
    isModalShow.value = true;
    isLogin.value = true;
}

const openModalRegister = () => {
    emits('openModal')
    isModalShow.value = true;
    isLogin.value = false;
}

const email = ref("")
const password = ref("")
const fullname = ref("")
const konpassword= ref("")

const isFormValid = () => {
    return email.value.includes("@") && email.value.length > 5 && password.value.length > 6
}

const isRegisterFormValid = () => {

     return email.value.includes("@") && email.value.length > 5 && password.value.length > 6 && konpassword.value === password.value && fullname.value.length > 5
}

defineProps({
    color:"",
})

</script>

<template>
    <header :class="color" class="w-full flex h-[60px] items-center gap-x-6 px-6 sticky top-0">
        <figure>
            <figcaption class="font-bold">
                <img src="../assets/KA.png" width="40" alt="">
            </figcaption>
        </figure>
        <nav :class="color" class="flex justify-between items-center w-full">
            <div class="flex gap-x-4"> 
                <span class="text-white font-bold cursor-default"> | </span>
                <router-link to="/"> 
                    <span class="text-white font-bold">Home</span>
                </router-link>
                <router-link to="/about">
                    <span class="text-white font-bold">About</span>
                </router-link>
                <router-link to="/contact">
                    <span class="text-white font-bold">Contact</span>
                </router-link>
            </div>

            <div class="flex gap-x-4" v-if="isAuthenticated">     
                <span class="text-white font-bold cursor-default">{{ currentFullname }}</span>
                <span class="text-white font-bold cursor-default">  |  </span>
                <span @click="isLogout()" class="text-white font-bold cursor-pointer"> Logout</span>
            </div>   

            <div class="flex gap-x-4" v-else>     
                <span @click="openModalLogin()" class="text-white font-bold cursor-pointer">Login</span>
                <span class="text-white font-bold cursor-default">  |  </span>
                <span @click="openModalRegister()" class="text-white font-bold cursor-pointer"> Register</span>
            </div>            
        </nav>
    </header>
    <Modal  
    v-if="isModalShow" 
    @close-modal="isModalShow = false" 
    :title="isLogin ? 'Login' : 'Register'" 
    :btn-Text="isLogin ? 'Login' : 'Register'" 
    :isValid=" isLogin ? !isFormValid() : !isRegisterFormValid()" 
    @submit-modal="isLogin ? submitLogin() : submitRegister()"
    >
    
<form>
  <div class="mb-6" v-if="!isLogin">
    <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Nama Lengkap</label>
    <input v-model="fullname" type="text" id="name" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan Nama Lengkap" required="">
  </div>
  <div class="mb-6">
    <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Email</label>
    <input v-model="email" type="email" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan Email" required="">
  </div>
  <div class="mb-6">
    <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Password</label>
    <input v-model="password" type="password" id="password" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan Password" required="">
  </div>
  <div class="mb-6" v-if="!isLogin">
    <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Konfirmasi Password</label>
    <input v-model="konpassword" type="password" id="password" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan Konfirmasi Password" required="">
  </div>
  
  <div class="flex items-start mb-6" v-if="isLogin">
    <div class="flex items-center h-5">
      <input id="remember" type="checkbox" value="" class="w-4 h-4 bg-gray-50 rounded border border-gray-300 focus:ring-3 focus:ring-blue-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-blue-600 dark:ring-offset-gray-800" required="">
    </div>
    <label for="remember" class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">Remember me</label>
  </div>    
</form>

    </Modal>
</template>