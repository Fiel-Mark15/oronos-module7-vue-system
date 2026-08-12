<script setup>
defineProps({ books: Array, search: String, darkMode: Boolean })
const emit = defineEmits(['edit', 'remove'])
</script>

<template>
  <div class="rounded-2xl border overflow-hidden transition-all duration-300"
    :style="darkMode
      ? 'background: linear-gradient(135deg, rgba(255,255,255,0.05) 0%, rgba(255,255,255,0.02) 100%); border-color: rgba(255,255,255,0.1);'
      : 'background: white; border-color: #e5e7eb;'">

    <!-- Header -->
    <div class="flex items-center justify-between px-6 py-4 border-b"
      :style="darkMode ? 'border-color: rgba(255,255,255,0.08);' : 'border-color: #f3f4f6;'">
      <h2 class="text-sm font-semibold" :class="darkMode ? 'text-gray-300' : 'text-slate-700'">Book Records</h2>
      <span class="rounded-full px-3 py-0.5 text-xs font-medium"
        :style="darkMode
          ? 'background: rgba(255,255,255,0.1); color: rgba(255,255,255,0.6); border: 1px solid rgba(255,255,255,0.15);'
          : 'background: #eef2ff; color: #4f46e5;'">
        {{ books.length }} {{ books.length === 1 ? 'record' : 'records' }}
      </span>
    </div>

    <!-- Empty State -->
    <div v-if="books.length === 0" class="flex flex-col items-center justify-center px-6 py-16 text-center">
      <svg xmlns="http://www.w3.org/2000/svg" class="mb-3 h-12 w-12" :class="darkMode ? 'text-gray-700' : 'text-gray-200'" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
          d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
      </svg>
      <p class="text-sm font-medium" :class="darkMode ? 'text-gray-500' : 'text-slate-400'">
        {{ search ? 'No books match your search.' : 'No book records yet. Click Add Book to get started.' }}
      </p>
    </div>

    <!-- Desktop Table -->
    <div v-else class="hidden overflow-x-auto sm:block">
      <table class="w-full text-sm">
        <thead>
          <tr :style="darkMode ? 'background: rgba(0,0,0,0.3);' : 'background: #f9fafb;'">
            <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider" :class="darkMode ? 'text-gray-500' : 'text-slate-500'">Title</th>
            <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider" :class="darkMode ? 'text-gray-500' : 'text-slate-500'">Author</th>
            <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider" :class="darkMode ? 'text-gray-500' : 'text-slate-500'">Category</th>
            <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider" :class="darkMode ? 'text-gray-500' : 'text-slate-500'">Status</th>
            <th class="px-6 py-3 text-left text-xs font-semibold uppercase tracking-wider" :class="darkMode ? 'text-gray-500' : 'text-slate-500'">Borrowed By</th>
            <th class="px-6 py-3 text-right text-xs font-semibold uppercase tracking-wider" :class="darkMode ? 'text-gray-500' : 'text-slate-500'">Actions</th>
          </tr>
        </thead>
        <tbody>
          <transition-group name="row">
            <tr v-for="book in books" :key="book.id"
              class="transition-all duration-200 border-b"
              :style="darkMode ? 'border-color: rgba(255,255,255,0.05);' : 'border-color: #f3f4f6;'"
              @mouseover="e => e.currentTarget.style.background = darkMode ? 'rgba(255,255,255,0.03)' : '#f9fafb'"
              @mouseleave="e => e.currentTarget.style.background = 'transparent'">
              <td class="px-6 py-4 font-semibold max-w-[180px] truncate" :class="darkMode ? 'text-white' : 'text-slate-800'">{{ book.title }}</td>
              <td class="px-6 py-4" :class="darkMode ? 'text-gray-400' : 'text-slate-600'">{{ book.author }}</td>
              <td class="px-6 py-4">
                <span class="rounded-full px-2.5 py-0.5 text-xs font-medium"
                  :style="darkMode
                    ? 'background: rgba(255,255,255,0.08); color: rgba(255,255,255,0.6);'
                    : 'background: #f1f5f9; color: #475569;'">
                  {{ book.category }}
                </span>
              </td>
              <td class="px-6 py-4">
                <span class="rounded-full px-2.5 py-0.5 text-xs font-bold"
                  :style="book.status === 'Available'
                    ? 'background: rgba(34,197,94,0.15); color: #86efac; border: 1px solid rgba(34,197,94,0.3);'
                    : 'background: rgba(251,191,36,0.15); color: #fcd34d; border: 1px solid rgba(251,191,36,0.3);'">
                  {{ book.status }}
                </span>
              </td>
              <td class="px-6 py-4 text-sm" :class="darkMode ? 'text-gray-500' : 'text-slate-400'">{{ book.borrowedBy || '—' }}</td>
              <td class="px-6 py-4 text-right">
                <div class="flex justify-end gap-2">
                  <button @click="emit('edit', book)"
                    class="rounded-lg px-3 py-1.5 text-xs font-semibold transition-all duration-200 hover:scale-105 active:scale-95"
                    style="background: rgba(255,255,255,0.08); color: rgba(255,255,255,0.8); border: 1px solid rgba(255,255,255,0.15);">
                    Edit
                  </button>
                  <button @click="emit('remove', book.id)"
                    class="rounded-lg px-3 py-1.5 text-xs font-semibold transition-all duration-200 hover:scale-105 active:scale-95"
                    style="background: rgba(239,68,68,0.1); color: #fca5a5; border: 1px solid rgba(239,68,68,0.3);">
                    Delete
                  </button>
                </div>
              </td>
            </tr>
          </transition-group>
        </tbody>
      </table>
    </div>

    <!-- Mobile Cards -->
    <div v-if="books.length > 0" class="divide-y sm:hidden"
      :style="darkMode ? 'border-color: rgba(255,255,255,0.06);' : ''">
      <transition-group name="row">
        <div v-for="book in books" :key="book.id" class="p-4 transition-all duration-300">
          <div class="flex items-start justify-between gap-2">
            <div class="flex-1 min-w-0">
              <p class="font-semibold truncate" :class="darkMode ? 'text-white' : 'text-slate-800'">{{ book.title }}</p>
              <p class="text-sm mt-0.5" :class="darkMode ? 'text-gray-400' : 'text-slate-500'">{{ book.author }}</p>
              <div class="mt-2 flex flex-wrap gap-2">
                <span class="rounded-full px-2 py-0.5 text-xs"
                  :style="darkMode ? 'background:rgba(255,255,255,0.08);color:rgba(255,255,255,0.5);' : 'background:#f1f5f9;color:#475569;'">
                  {{ book.category }}
                </span>
                <span class="rounded-full px-2 py-0.5 text-xs font-bold"
                  :style="book.status === 'Available'
                    ? 'background:rgba(34,197,94,0.15);color:#86efac;'
                    : 'background:rgba(251,191,36,0.15);color:#fcd34d;'">
                  {{ book.status }}
                </span>
                <span v-if="book.borrowedBy" class="text-xs" :class="darkMode ? 'text-gray-500' : 'text-slate-400'">by {{ book.borrowedBy }}</span>
              </div>
            </div>
            <div class="flex shrink-0 gap-2">
              <button @click="emit('edit', book)"
                class="rounded-lg px-3 py-1 text-xs font-semibold transition-all hover:scale-105"
                style="background:rgba(255,255,255,0.08);color:rgba(255,255,255,0.8);border:1px solid rgba(255,255,255,0.15);">
                Edit
              </button>
              <button @click="emit('remove', book.id)"
                class="rounded-lg px-3 py-1 text-xs font-semibold transition-all hover:scale-105"
                style="background:rgba(239,68,68,0.1);color:#fca5a5;border:1px solid rgba(239,68,68,0.3);">
                Delete
              </button>
            </div>
          </div>
        </div>
      </transition-group>
    </div>
  </div>
</template>

<style scoped>
.row-enter-active { transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1); }
.row-leave-active { transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1); }
.row-enter-from { opacity: 0; transform: translateY(-10px) scale(0.98); }
.row-leave-to { opacity: 0; transform: translateX(20px) scale(0.96); }
</style>
