<script setup>
import { logout } from '../services/auth.js';
import { useRouter } from 'vue-router';
import { useAuth } from '../composition/useAuth.js';
import { ref } from 'vue';

// Componentes
import Imagen from './Imagen.vue';

const { user } = useAuth();
const { handleLogout, toggleMenu, isMenuExpanded } = useLogout();

function useLogout() {
    const router = useRouter();
    
    function handleLogout() {
        logout();
        router.push('/iniciar-sesion');
    }

    const isMenuExpanded = ref(false);

    function toggleMenu() {
        isMenuExpanded.value = !isMenuExpanded.value;
    }

    return {
        handleLogout,
        toggleMenu,
        isMenuExpanded,
    };
}
</script>

<template>
    <div class="col-auto col-md-3 col-xl-2 px-sm-2 px-0 bg-admin rounded-0" :class="isMenuExpanded ? 'expanded' : 'collapsed'">
        <div class="d-flex flex-column align-items-center align-items-sm-start px-3 pt-2 min-vh-100 sticky-top">
            <button class="btn btn-link text-white ms-2" @click="toggleMenu()">
                <i class="fs-3 bi-list"></i>
            </button>
            <router-link to="/admin/panel/pricing" class="d-flex align-items-center ms-3 pb-3 mb-md-0 me-md-auto text-decoration-none">
                <img src="../assets/logo/logo.png" alt="Logo de Cloudnet" class="mt-2">
                <h1 class="fs-4 fw-bold d-none d-sm-inline mt-3 ps-1 text-white">Cloudnet</h1>
            </router-link>
            <ul class="nav nav-pills flex-column mb-sm-auto mb-0 align-items-center align-items-sm-start" id="menu">
                <li class="nav-item">
                    <router-link to="/admin/panel/pricing" class="nav-link align-middle px-0 text-white">
                        <i class="fs-4 bi-house"></i> <span class="ms-1 d-none d-sm-inline"> Inicio</span>
                    </router-link>
                </li>
                <li class="w-100 nav-item">
                    <router-link to="/admin/panel/usuarios" class="nav-link align-middle px-0 text-white">
                        <i class="fs-4 bi-person-circle"></i><span class="ms-1 d-none d-sm-inline"> Usuarios</span>
                    </router-link>
                </li>
                <li class="w-100 nav-item">
                    <router-link to="/admin/panel/chats" class="nav-link align-middle px-0 text-white">
                        <i class="fs-4 bi-envelope"></i><span class="ms-1 d-none d-sm-inline"> Mensajes</span>
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link to="/" class="nav-link px-0 align-middle text-white">
                        <i class="fs-4 bi-arrow-left-circle"></i> <span class="ms-1 d-none d-sm-inline">Volver al sitio</span>
                    </router-link>
                </li>
            </ul>
            <hr>
            <div class="dropdown px-2 pb-4">
                <a href="#" class="d-flex align-items-center text-dark text-decoration-none dropdown-toggle"
                    id="dropdownUsuario" data-bs-toggle="dropdown" aria-expanded="false">
                    <Imagen :src="user.photoURL" width="30" height="30" class="rounded-circle"></Imagen>
                    <span class="d-none d-sm-inline mx-2 text-white">{{ user.name }} {{ user.surname }}(admin)</span>
                </a>
                <ul class="dropdown-menu dropdown-menu-dark text-small shadow">
                    <li><router-link class="dropdown-item" to="/perfil">Mi perfil</router-link></li>
                    <form action="#" method="post" @submit.prevent="handleLogout">
                        <li class="nav-item"><button class="dropdown-item" href="#">Cerrar sesión</button></li>
                    </form>
                </ul>
            </div>
        </div>
    </div>
</template>

