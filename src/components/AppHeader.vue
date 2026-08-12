<script setup>
defineProps({ darkMode: Boolean })
defineEmits(['toggleTheme'])
</script>

<template>
  <header class="sticky top-0 z-50 transition-all duration-500"
    :style="darkMode
      ? 'background: linear-gradient(90deg, #000000 0%, #0f172a 50%, #000000 100%); border-bottom: 1px solid rgba(255,255,255,0.07);'
      : 'background: linear-gradient(90deg, #0f172a 0%, #1e293b 50%, #0f172a 100%); border-bottom: 1px solid rgba(0,0,0,0.2);'">
    <div class="mx-auto max-w-6xl px-4 py-4 flex items-center justify-between">

      <!-- Left: Logo + Title -->
      <div class="flex items-center gap-3">
        <div class="flex h-10 w-10 shrink-0 items-center justify-center rounded-xl"
          style="background: linear-gradient(135deg, rgba(255,255,255,0.15), rgba(255,255,255,0.04)); border: 1px solid rgba(255,255,255,0.12);">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
              d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
          </svg>
        </div>
        <div>
          <h1 class="text-base font-bold text-white leading-tight tracking-tight">Library Management System</h1>
          <p class="text-xs" style="color: rgba(255,255,255,0.38);">Book Records · Frontend Prototype · Module 7</p>
        </div>
      </div>

      <!-- Right: Badge + Theme Toggle -->
      <div class="flex items-center gap-3">
        <span class="hidden sm:block rounded-full px-3 py-1 text-xs font-medium"
          style="background: rgba(255,255,255,0.07); color: rgba(255,255,255,0.5); border: 1px solid rgba(255,255,255,0.1);">
          Vue.js + Tailwind CSS
        </span>

        <!-- Dark / Light Toggle -->
        <button @click="$emit('toggleTheme')"
          class="flex items-center gap-2 rounded-xl px-3 py-2 text-xs font-semibold transition-all duration-300 hover:scale-105 active:scale-95"
          style="background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.12); color: rgba(255,255,255,0.7);">
          <transition name="icon-switch" mode="out-in">
            <span v-if="darkMode" key="sun" class="text-base">☀️</span>
            <span v-else key="moon" class="text-base">🌙</span>
          </transition>
          <span class="hidden sm:inline">{{ darkMode ? 'Light' : 'Dark' }}</span>
        </button>
      </div>
    </div>
  </header>
</template>

<style scoped>
.icon-switch-enter-active, .icon-switch-leave-active { transition: all 0.2s ease; }
.icon-switch-enter-from { opacity: 0; transform: rotate(-90deg) scale(0.5); }
.icon-switch-leave-to   { opacity: 0; transform: rotate(90deg) scale(0.5); }
</style>
