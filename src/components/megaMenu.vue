<template>
  <div class="min-h-full bg-gray-200 text-gray-900 dark:text-gray-200">
    <!-- Navbar -->
    <Disclosure as="nav" class="bg-gray-800 dark:bg-gray-900" v-slot="{ open }">
      <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div class="flex h-16 items-center justify-between">
          <div class="flex items-center">
            <div class="shrink-0">
              <img class="size-8" src="https://tailwindui.com/plus-assets/img/logos/mark.svg?color=indigo&shade=500" alt="Your Company" />
            </div>
            <div class="hidden md:block">
              <div class="ml-10 flex items-baseline space-x-4">
                <a v-for="item in navigation" :key="item.name" :href="item.href"
                  :class="[item.current ? 'bg-gray-900 text-white dark:bg-gray-700' : 'text-gray-300 dark:text-gray-200 hover:bg-gray-700 hover:dark:bg-gray-800 hover:text-white', 'rounded-md px-3 py-2 text-sm font-medium']"
                  :aria-current="item.current ? 'page' : undefined">{{ item.name }}</a>
              </div>
            </div>
          </div>
          
          <!-- Tombol Mode Gelap/Terang
          <button @click="toggleDarkMode" class="ml-4 p-2 rounded-full bg-gray-800 dark:bg-gray-700 text-gray-400 hover:text-white dark:text-gray-300">
            <SunIcon v-if="isDark" class="size-6" />
            <MoonIcon v-else class="size-6" />
          </button> -->

          <div class="hidden md:block">
            <div class="ml-4 flex items-center md:ml-6">
              <button type="button" class="relative rounded-full bg-gray-800 dark:bg-gray-700 p-1 text-gray-400 hover:text-white focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-gray-800">
                <span class="sr-only">View notifications</span>
                <BellIcon class="size-6" aria-hidden="true" />
              </button>
              <Menu as="div" class="relative ml-3">
                <div>
                  <MenuButton class="relative flex max-w-xs items-center rounded-full bg-gray-800 dark:bg-gray-700 text-sm focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-gray-800">
                    <span class="sr-only">Open user menu</span>
                    <img class="size-8 rounded-full" :src="user.imageUrl" alt="" />
                  </MenuButton>
                </div>
                <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100"
                  leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
                  <MenuItems class="absolute right-0 z-10 mt-2 w-48 origin-top-right rounded-md bg-white dark:bg-gray-800 py-1 shadow-lg ring-1 ring-black/5">
                    <MenuItem v-for="item in userNavigation" :key="item.name" v-slot="{ active }">
                      <a :href="item.href" :class="[active ? 'bg-gray-100 dark:bg-gray-700' : '', 'block px-4 py-2 text-sm text-gray-700 dark:text-gray-200']">{{ item.name }}</a>
                    </MenuItem>
                  </MenuItems>
                </transition>
              </Menu>
            </div>
          </div>
        </div>
      </div>
    </Disclosure>

    <!-- Header -->
    <header class="bg-white dark:bg-gray-800 shadow-sm">
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        <h1 class="text-3xl font-bold tracking-tight text-gray-900 dark:text-white">Dashboard</h1>
      </div>
    </header>

    <!-- Main Content -->
    <main>
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        <ButtonBase label="Klik di sini" @click="handleClick" />
        <!-- <ButtonLight label="Klik ini aja" variant="primary" size="lg" @click="handleClick"/> -->
      </div>
    </main>
  </div>
</template>

<script setup>
// import { ref, onMounted } from 'vue'
import { Disclosure, Menu, MenuButton, MenuItem, MenuItems } from '@headlessui/vue'
import { BellIcon } from '@heroicons/vue/24/outline'
// import { SunIcon, MoonIcon } from '@heroicons/vue/24/solid' // Tambahkan ikon matahari dan bulan
import ButtonBase from './buttonBase.vue'
// import ButtonLight from './buttonLight.vue'

// // State untuk mode gelap
// const isDark = ref(localStorage.getItem('theme') === 'dark')

// // Fungsi untuk mengganti tema
// const toggleDarkMode = () => {
//   isDark.value = !isDark.value
//   document.documentElement.classList.toggle('dark', isDark.value)
//   localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
// }

// // Cek dan set tema saat halaman dimuat
// onMounted(() => {
//   if (localStorage.getItem('theme') === 'dark') {
//     document.documentElement.classList.add('dark')
//     isDark.value = true
//   }
// })

// Data pengguna
const user = {
  name: 'Tom Cook',
  email: 'tom@example.com',
  imageUrl: 'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80'
}

// Navigasi
const navigation = [
  { name: 'Dashboard', href: '#', current: true },
  { name: 'History', href: '#', current: false },
  { name: 'Register', href: '#', current: false },
  { name: 'Certificate', href: '#', current: false },
]

// Navigasi pengguna
const userNavigation = [
  { name: 'Your Profile', href: '#' },
  { name: 'Settings', href: '#' },
  { name: 'Sign out', href: '#' }
]

// Fungsi tombol klik
const handleClick = () => {
  alert('Button clicked!')
}
</script>
