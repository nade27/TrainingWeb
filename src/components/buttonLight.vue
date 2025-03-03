<template>
    <button
      :class="buttonClasses"
      @click="handleClick"
    >
    {{ label }}
    </button>
  </template>
  
  <script setup>
  import { computed } from 'vue'
  
  const props = defineProps({
    variant: {
      type: String,
      default: 'primary', // primary | secondary | danger | success
    },
    size: {
      type: String,
      default: 'md', // sm | md | lg
    },
    rounded: {
      type: Boolean,
      default: true,
    },
  })
  
  const emit = defineEmits(['click'])
  
  const handleClick = (event) => {
    emit('click', event)
  }
  
  // Class Tailwind yang akan diterapkan berdasarkan props
  const buttonClasses = computed(() => {
    const baseClasses = 'transition duration-300 font-semibold uppercase tracking-wide border px-4 py-2 shadow-md focus:outline-none focus:ring'
    const sizeClasses = {
      sm: 'text-xs px-3 py-1',
      md: 'text-sm px-4 py-2',
      lg: 'text-lg px-15 py-5',
    }[props.size]
  
    const variantClasses = {
      primary: 'bg-blue-500 text-white border-blue-500 hover:bg-blue-600 hover:shadow-blue-500/50 focus:ring-blue-300',
      secondary: 'bg-gray-500 text-white border-gray-500 hover:bg-gray-600 hover:shadow-gray-500/50 focus:ring-gray-300',
      danger: 'bg-red-500 text-white border-red-500 hover:bg-red-600 hover:shadow-red-500/50 focus:ring-red-300',
      success: 'bg-green-500 text-white border-green-500 hover:bg-green-600 hover:shadow-green-500/50 focus:ring-green-300',
    }[props.variant]
  
    const roundedClass = props.rounded ? 'rounded-full' : 'rounded-md'
  
    return `${baseClasses} ${sizeClasses} ${variantClasses} ${roundedClass}`
  })
  </script>
  