<script setup>
defineProps({ darkMode: Boolean })
</script>

<template>
  <footer class="mt-10 transition-all duration-500"
    :style="darkMode
      ? 'background: linear-gradient(90deg,#000 0%,#0f172a 50%,#000 100%); border-top: 1px solid rgba(255,255,255,0.07);'
      : 'background: linear-gradient(90deg,#0f172a 0%,#1e293b 50%,#0f172a 100%); border-top: 1px solid rgba(0,0,0,0.15);'">
    <div class="mx-auto max-w-6xl px-4 py-5 flex flex-col items-center gap-1 text-center sm:flex-row sm:justify-between">
      <p class="text-sm font-medium" style="color: rgba(255,255,255,0.45);">
        Library Management System — Book Records Module
      </p>
      <p class="text-xs" style="color: rgba(255,255,255,0.25);">
        Fiel Mark Oronos · BSCS 3A · Software Engineering 1 · Module 7
      </p>
    </div>
  </footer>
</template>
