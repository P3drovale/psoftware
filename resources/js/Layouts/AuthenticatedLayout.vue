<script setup>
import { ref } from 'vue'
import ApplicationLogo from '@/Components/ApplicationLogo.vue'
import Dropdown from '@/Components/Dropdown.vue'
import DropdownLink from '@/Components/DropdownLink.vue'
import NavLink from '@/Components/NavLink.vue'
import { Link } from '@inertiajs/vue3'

const showSidebar = ref(true)
</script>

<template>
    <div class="flex h-screen bg-gray-100 gap-1 pr-1">
        <!-- Sidebar (desplegable) -->
        <aside class="bg-white border-r shadow-md transition-all duration-300 ease-in-out mr-1 rounded-r-lg"
            :class="showSidebar ? 'w-64' : 'w-0 overflow-hidden'">
            <div class="p-4 border-b flex items-center justify-between">
                <Link :href="route('dashboard')" class="flex items-center gap-2">
                <ApplicationLogo class="h-8 w-auto" />
                <span class="font-semibold text-lg text-gray-800" v-show="showSidebar">Mi Panel</span>
                </Link>
                <!-- Botón cerrar sidebar -->
                <button class="text-gray-600 hover:text-gray-800" @click="showSidebar = false" v-if="showSidebar">
                    ☰
                </button>
            </div>

            <!-- Contenedor centrado -->
            <div class="flex-1 py-2 flex items-center justify-center">
                <nav v-show="showSidebar">
                    <ul class="space-y-2">
                        <li>
                            <NavLink :href="route('dashboard')" :active="route().current('dashboard')"
                                class="flex items-center px-6 py-3 text-lg font-semibold text-gray-700 hover:bg-gray-100 hover:text-blue-600 transition duration-200">
                                <FontAwesomeIcon :icon="['fas', 'tachometer-alt']" />
                                <span class="px-2">Dashboard</span>
                            </NavLink>
                        </li>
                        <!-- Agrega más ítems aquí si deseas -->
                    </ul>
                </nav>
            </div>
        </aside>

        <!-- Contenido principal -->
        <div class="flex flex-col flex-1">
            <!-- Header -->
            <header class="bg-white border-b p-4 flex justify-between items-center">
                <!-- Botón hamburguesa para mostrar sidebar -->
                <button class="text-gray-600 hover:text-gray-800" @click="showSidebar = true" v-if="!showSidebar">
                    ☰
                </button>

                <!-- Espaciador para centrar título o dejar header vacío -->
                <div class="flex-1"></div>

                <!-- Perfil / Logout -->
                <Dropdown align="right" width="48">
                    <template #trigger>
                        <span class="inline-flex rounded-md">
                            <button type="button"
                                class="inline-flex items-center rounded-md bg-white px-3 py-2 text-sm font-medium text-gray-600 hover:text-gray-800">
                                {{ $page.props.auth.user.name }}
                                <svg class="ml-2 h-4 w-4" xmlns="http://www.w3.org/2000/svg" fill="none"
                                    viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M19 9l-7 7-7-7" />
                                </svg>
                            </button>
                        </span>
                    </template>

                    <template #content>
                        <DropdownLink :href="route('profile.edit')">
                            <FontAwesomeIcon :icon="['fas', 'user']" />
                            <span class="px-2">Perfil</span>
                        </DropdownLink>
                        <DropdownLink :href="route('logout')" method="post" as="button">Cerrar sesión</DropdownLink>
                    </template>
                </Dropdown>
            </header>

            <!-- Header personalizado de la página (si lo hay) -->
            <div v-if="$slots.header" class="bg-white border-b">
                <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
                    <slot name="header" />
                </div>
            </div>

            <!-- Contenido principal -->
            <main class="p-6 flex-1 overflow-y-auto">
                <slot />
            </main>
        </div>
    </div>
</template>
