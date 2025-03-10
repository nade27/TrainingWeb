<template>
    <!-- Main Content -->
    <main>
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        <ButtonBase label="Klik di sini" @click="handleClick" />
        <!-- <ButtonLight label="Klik ini aja" variant="primary" size="lg" @click="handleClick"/> -->
      </div>
    </main>
</template>

<script setup>
// import { ref, onMounted } from 'vue'
// import { SunIcon, MoonIcon } from '@heroicons/vue/24/solid' // Tambahkan ikon matahari dan bulan
import ButtonBase from './buttonBase.vue'

// Fungsi tombol klik
const handleClick = () => {
  alert('Button clicked!')
}
</script>
