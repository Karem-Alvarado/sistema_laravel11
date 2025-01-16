<script setup>
    import { ref, onMounted } from 'vue';
    import { Head, Link, router } from '@inertiajs/vue3';
    import ApplicationMark from '@/Components/ApplicationMark.vue';
    import Banner from '@/Components/Banner.vue';
    import Dropdown from '@/Components/Dropdown.vue';
    import DropdownLink from '@/Components/DropdownLink.vue';
    import { usePage } from '@inertiajs/vue3';

    const page = usePage();

    defineProps({
        title: String,
    });

    const showingNavigationDropdown = ref(false);
    const isSidebarCollapsed = ref(false); // Variable para colapsar el sidebar

    const switchToTeam = (team) => {
        router.put(route('current-team.update'), {
            team_id: team.id,
        }, {
            preserveState: false,
        });
    };

    const logout = () => {
        router.post(route('logout'));
    };

    // Manejo del modo oscuro/claro
    const isDarkMode = ref(false);

    const toggleDarkMode = () => {
        isDarkMode.value = !isDarkMode.value;

        if (isDarkMode.value) {
            document.documentElement.classList.add('dark');
            localStorage.setItem('theme', 'dark');
        } else {
            document.documentElement.classList.remove('dark');
            localStorage.setItem('theme', 'light');
        }
    };

    // Sincronizar tema inicial con localStorage
    onMounted(() => {
        const storedTheme = localStorage.getItem('theme');
        if (storedTheme === 'dark' || (!storedTheme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('dark');
            isDarkMode.value = true;
        } else {
            document.documentElement.classList.remove('dark');
            isDarkMode.value = false;
        }
    });
    // Opciones del sidebar y hamburguesa
    const sidebarOptions = [
        { name: 'Home', icon: 'M3.75 12l8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125a1.125 1.125 0 001.125 1.125h10.125', route: route('skill'), isActive: () => route().current('skill') },
        { name: 'Dashboard', icon: 'M3 13.125C3 12.504 3.504 12 4.125 12h2.25c.621 0 1.125.504 1.125 1.125v6.75C7.5 20.496 6.996 21 6.375 21h-2.25A1.125 1.125 0 013 19.875v-6.75zM9.75 8.625c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125v11.25c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V8.625zM16.5 4.125c0-.621.504-1.125 1.125-1.125h2.25C20.496 3 21 3.504 21 4.125v15.75c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V4.125z', route: route('plain'), isActive: () => route().current('plain') },
    ];
</script>
<template>
    <!-- <div class="min-h-screen flex flex-col bg-gray-100 dark:bg-gray-900 dark:border-gray-700 h-64 custom-scrollbar overflow-y-auto scrollbar-thumb-scroll-thumb scrollbar-track-scroll-track scrollbar-thumb-rounded scrollbar-thin hover:scrollbar-thumb-scroll-thumb-hover"> -->
    <div class="min-h-screen fixed left-0 w-full flex flex-col bg-gray-100 dark:bg-gray-900 dark:border-gray-700">
        <!-- Head -->
        <Head :title="title" />
        <!-- Banner -->
        <Banner />
        <!-- Navbar -->
        <nav class="bg-white border-b border-white dark:bg-gray-800 dark:border-gray-800 z-10">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between h-16">
                    <div class="flex">
                        <!-- Logo -->
                        <div class="shrink-0 flex items-center">
                            <Link :href="route('skill')">
                                <ApplicationMark class="block h-9 w-auto" />
                            </Link>
                        </div>
                        <!-- Botón para colapsar/expandir el sidebar -->
                        <button
                            class="ml-4 p-2 rounded-lg dark:text-gray-200 hover:text-indigo-500 dark:hover:text-indigo-500 hidden sm:block"
                            @click="isSidebarCollapsed = !isSidebarCollapsed">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 12h16.5M3.75 6h16.5M3.75 18h16.5" />
                            </svg>
                        </button>
                    </div>

                    <div class="hidden sm:flex sm:items-center sm:ms-6">
                        <!-- Botón para cambiar modo oscuro/claro -->
                        <button
                            aria-label="theme switching"
                            type="button"
                            @click="toggleDarkMode"
                            class="group flex max-w-[2.25rem] border mx-2 border-gray-200/40 dark:border-gray-700/40 bg-gray-100/20 dark:bg-gray-800/20 rounded-lg h-9 w-9 items-center justify-center">
                            <svg
                                v-if="!isDarkMode"
                                aria-hidden="true"
                                xmlns="http://www.w3.org/2000/svg"
                                viewBox="0 0 24 24"
                                fill="currentColor"
                                class="w-4 h-4 text-gray-700 duration-500 group-hover:text-gray-900 group-hover:rotate-[360deg]"
                            >
                                <path
                                    fill-rule="evenodd"
                                    d="M9.528 1.718a.75.75 0 01.162.819A8.97 8.97 0 009 6a9 9 0 009 9 8.97 8.97 0 003.463-.69.75.75 0 01.981.98 10.503 10.503 0 01-9.694 6.46c-5.799 0-10.5-4.701-10.5-10.5 0-4.368 2.667-8.112 6.46-9.694a.75.75 0 01.818.162z"
                                    clip-rule="evenodd"
                                />
                            </svg>
                            <svg
                                v-if="isDarkMode"
                                aria-hidden="true"
                                xmlns="http://www.w3.org/2000/svg"
                                viewBox="0 0 24 24"
                                fill="currentColor"
                                class="w-5 h-5 text-white duration-300 group-hover:rotate-180"
                            >
                                <path
                                    d="M12 2.25a.75.75 0 01.75.75v2.25a.75.75 0 01-1.5 0V3a.75.75 0 01.75-.75zM7.5 12a4.5 4.5 0 119 0 4.5 4.5 0 01-9 0zM18.894 6.166a.75.75 0 00-1.06-1.06l-1.591 1.59a.75.75 0 101.06 1.061l1.591-1.59zM21.75 12a.75.75 0 01-.75.75h-2.25a.75.75 0 010-1.5H21a.75.75 0 01.75.75zM17.834 18.894a.75.75 0 001.06-1.06l-1.59-1.591a.75.75 0 10-1.061 1.06l1.59 1.591zM12 18a.75.75 0 01.75.75V21a.75.75 0 01-1.5 0v-2.25A.75.75 0 0112 18zM7.758 17.303a.75.75 0 00-1.061-1.06l-1.591 1.59a.75.75 0 001.06 1.061l1.591-1.59zM6 12a.75.75 0 01-.75.75H3a.75.75 0 010-1.5h2.25A.75.75 0 016 12zM6.697 7.757a.75.75 0 001.06-1.06l-1.59-1.591a.75.75 0 00-1.061 1.06l1.59 1.591z"
                                />
                            </svg>
                        </button>
                        <!-- Settings Dropdown -->
                        <Dropdown align="right" width="48">
                            <template #trigger>
                                <button v-if="$page.props.jetstream.managesProfilePhotos"
                                    class="flex text-sm border-2 border-transparent rounded-full focus:outline-none focus:border-gray-300 transition">
                                    <img class="size-8 rounded-full object-cover"
                                        :src="$page.props.auth.user.profile_photo_url"
                                        :alt="$page.props.auth.user.name">
                                </button>
                                <span v-else class="inline-flex rounded-md">
                                    <button
                                        class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md text-gray-500 bg-white hover:text-gray-700 focus:outline-none focus:bg-gray-50 dark:bg-gray-800 dark:text-gray-400 dark:hover:text-gray-300 active:bg-gray-50 transition ease-in-out duration-150">
                                        {{ $page.props.auth.user.name }}
                                        <svg class="ms-2 -me-0.5 size-4" xmlns="http://www.w3.org/2000/svg" fill="none"
                                            viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round"
                                                d="M19.5 8.25l-7.5 7.5-7.5-7.5" />
                                        </svg>
                                    </button>
                                </span>
                            </template>
                            <template #content>
                                <div class="block px-4 py-2 text-xs text-gray-400">
                                    Administrar cuenta
                                </div>
                                <!-- <DropdownLink :href="route('profile.show')">Perfil</DropdownLink> -->
                                <DropdownLink :href="route('profile.show')">Perfil</DropdownLink>
                                <DropdownLink as="button" @click="logout">Salir</DropdownLink>
                                <!-- <form @submit.prevent="logout">
                                    <DropdownLink as="button">Salir</DropdownLink>
                                </form> -->
                            </template>
                        </Dropdown>
                    </div>
                    <!-- Hamburger for Small Screens -->
                    <div class="-me-2 flex items-center sm:hidden">
                        <button
                            class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:bg-gray-100 focus:text-gray-500 transition duration-150 ease-in-out dark:hover:bg-gray-600 dark:text-gray-400 dark:hover:text-gray-300"
                            @click="showingNavigationDropdown = !showingNavigationDropdown">
                            <svg class="size-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
                                <path
                                    :class="{ 'hidden': showingNavigationDropdown, 'inline-flex': !showingNavigationDropdown }"
                                    stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M4 6h16M4 12h16M4 18h16" />
                                <path
                                    :class="{ 'hidden': !showingNavigationDropdown, 'inline-flex': showingNavigationDropdown }"
                                    stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Responsive Dropdown Menu -->
            <div :class="{ 'block': showingNavigationDropdown, 'hidden': !showingNavigationDropdown }"
                class="absolute left-0 w-full h-full bg-white z-50 sm:hidden">
                <nav class="px-4 py-3 space-y-1 bg-white border-t border-gray-200">
                    <!-- <Link href="#" class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:bg-gray-100">
                        Home
                    </Link>
                    <Link href="#" class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:bg-gray-100">
                        Dashboard
                    </Link> -->
                    <!-- Hamburger for Small Screens -->
                    <div v-for="option in sidebarOptions" :key="option.name">
                        <Link :href="option.route"
                            :class="[
                                'flex items-center px-3 py-2 text-gray-600 hover:bg-gray-100 rounded-lg dark:text-gray-300',
                                option.isActive() ? 'bg-indigo-50 text-indigo-600 dark:bg-gray-100 dark:text-indigo-600' : 'text-gray-600 hover:bg-gray-50 dark:text-gray-300 dark:hover:bg-gray-600'
                            ]">
                            <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke-width="1.5" stroke="currentColor" xmlns="http://www.w3.org/2000/svg" :d="option.icon">
                               <path :d="option.icon" />
                            </svg>
                            <span class="ml-2 text-sm">{{ option.name }}</span>
                        </Link>
                    </div>
                </nav>
                <!-- Hamburguesa (Responsive Menu) -->
                <!-- <nav class="bg-white border-b border-white dark:bg-gray-800 dark:border-gray-800 z-10">
                    <div v-if="showingNavigationDropdown" class="block sm:hidden">
                        <div v-for="option in sidebarOptions" :key="option.name">
                            <Link :href="option.route" class="block px-4 py-2 text-gray-700 dark:text-gray-200">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-6 h-6 inline-block">
                                    <path :d="option.icon" />
                                </svg>
                                {{ option.name }}
                            </Link>
                        </div>
                    </div>
                </nav> -->

                <!-- Account Section -->
                <div class="pt-4 pb-1 border-t border-gray-200">
                    <div class="flex items-center px-4">
                        <div v-if="$page.props.jetstream.managesProfilePhotos" class="shrink-0 me-3">
                            <img class="size-10 rounded-full object-cover"
                                :src="$page.props.auth.user.profile_photo_url"
                                :alt="$page.props.auth.user.name">
                        </div>
                        <div>
                            <div class="font-medium text-base text-gray-800">
                                {{ $page.props.auth.user.name }}
                            </div>
                            <div class="font-medium text-sm text-gray-500">
                                {{ $page.props.auth.user.email }}
                            </div>
                        </div>
                    </div>

                    <div class="mt-3 space-y-1">
                        <!-- Authentication -->
                        <form method="POST" @submit.prevent="logout">
                            <Link href="#" class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:bg-gray-100">
                                Salir
                            </Link>
                        </form>
                    </div>
                </div>
            </div>
        </nav>
        <div class="flex flex-1">
            <!-- Sidebar -->
            <aside
                :class="[isSidebarCollapsed
                        ? 'flex flex-col items-center w-16 py-3 bg-white border-white border-r dark:bg-gray-800 dark:border-gray-800'
                        : 'hidden sm:flex flex-col w-50 px-4 py-3 bg-white border-white border-r dark:bg-gray-800 dark:border-gray-800',
                    'hidden sm:flex' // Oculta en pantallas pequeñas y muestra en pantallas más grandes
                ]"
                class="h-[calc(100vh-4rem)] overflow-y-auto custom-scrollbar"
            >
                <nav
                    :class="isSidebarCollapsed
                        ? 'flex flex-col flex-1 space-y-6'
                        : 'flex-1 space-y-3'"
                >
                    <!-- Opciones del Sidebar -->
                    <!-- <div v-for="option in sidebarOptions" :key="option.name">
                        <a :href="option.route" 
                        class="flex items-center px-3 py-2 text-gray-600 hover:bg-indigo-50 hover:text-indigo-500 rounded-lg  dark:hover:bg-gray-600 dark:text-gray-400 dark:hover:text-gray-300">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-6 h-6 inline-block">
                                <path :d="option.icon" />
                            </svg>
                            <span v-if="!isSidebarCollapsed" class="ml-2 text-sm">{{ option.name }}</span>
                        </a>
                    </div> -->
                    <div v-for="option in sidebarOptions" :key="option.name">
                        <Link :href="option.route"
                            :class="[
                                'flex items-center px-3 py-2 text-gray-600 hover:bg-gray-100 rounded-lg dark:text-gray-300',
                                option.isActive() ? 'bg-indigo-50 text-indigo-600 dark:bg-gray-600 dark:hover:bg-gray-500' : 'text-gray-600 hover:bg-gray-50 dark:text-gray-300 dark:hover:bg-gray-400'
                            ]">
                            <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke-width="1.5" stroke="currentColor" xmlns="http://www.w3.org/2000/svg" :d="option.icon">
                               <path :d="option.icon" />
                            </svg>
                            <span v-if="!isSidebarCollapsed" class="ml-2 text-sm">{{ option.name }}</span>
                        </Link>
                    </div>
                    <!-- <a class="flex items-center px-3 py-2 text-gray-600 hover:bg-gray-100 rounded-lg dark:text-gray-300" href="#">
                        <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke-width="1.5" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M3.75 12l8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125a1.125 1.125 0 001.125 1.125h10.125" />
                        </svg>
                        <span v-if="!isSidebarCollapsed" class="ml-2 text-sm">Home</span>
                    </a>
                    <a class="flex items-center px-3 py-2 text-gray-600 hover:bg-gray-100 rounded-lg dark:text-gray-300" href="#">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M3 13.125C3 12.504 3.504 12 4.125 12h2.25c.621 0 1.125.504 1.125 1.125v6.75C7.5 20.496 6.996 21 6.375 21h-2.25A1.125 1.125 0 013 19.875v-6.75zM9.75 8.625c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125v11.25c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V8.625zM16.5 4.125c0-.621.504-1.125 1.125-1.125h2.25C20.496 3 21 3.504 21 4.125v15.75c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V4.125z" />
                        </svg>
                        <span v-if="!isSidebarCollapsed" class="ml-2 text-sm">Dashboard</span>
                    </a> -->
                </nav>
            </aside>
            <!-- Main Content -->
            <main class="flex-1 p-0 bg-white dark:bg-gray-800 h-[calc(100vh-4rem)]">
                <div class="bg-gray-100 dark:bg-gray-900 rounded-lg w-full h-full shadow-inner flex overflow-y-auto custom-scrollbar">
                    <slot />
                </div>
            </main>
        </div>
    </div>
</template>
<style>
.custom-scrollbar::-webkit-scrollbar {
    width: 8px;
    height: 8px;
}

.custom-scrollbar::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 8px;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
    background-color: #888;
    border-radius: 8px;
    border: 2px solid transparent;
}

.custom-scrollbar::-webkit-scrollbar-thumb:hover {
    background-color: #555;
}
</style>