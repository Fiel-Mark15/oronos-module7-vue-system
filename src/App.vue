<script setup>
import { ref, computed, onMounted } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import BookForm from './components/BookForm.vue'
import BookList from './components/BookList.vue'

const STORAGE_KEY = 'module7-records'
const darkMode = ref(true)

const records = ref([])
const searchTerm = ref('')
const editingId = ref(null)
const showForm = ref(false)
const feedback = ref({ text: '', type: '' })
const searchHighlight = ref(false)

function saveRecords() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(records.value))
}

onMounted(() => {
  const saved = localStorage.getItem(STORAGE_KEY)
  records.value = saved ? JSON.parse(saved) : []
  const savedTheme = localStorage.getItem('lms-theme')
  if (savedTheme) darkMode.value = savedTheme === 'dark'
})

function toggleTheme() {
  darkMode.value = !darkMode.value
  localStorage.setItem('lms-theme', darkMode.value ? 'dark' : 'light')
}

function showFeedback(text, type = 'success') {
  feedback.value = { text: '', type: '' }
  setTimeout(() => { feedback.value = { text, type } }, 50)
  setTimeout(() => { feedback.value = { text: '', type: '' } }, 3500)
}

function addRecord(newRecord) {
  records.value.push({ id: Date.now(), ...newRecord })
  saveRecords()
  showFeedback('✓ Book record added successfully.')
  showForm.value = false
}

function updateRecord(updated) {
  const index = records.value.findIndex(r => r.id === updated.id)
  if (index !== -1) records.value[index] = { ...updated }
  saveRecords()
  editingId.value = null
  showFeedback('✓ Book record updated successfully.')
  showForm.value = false
}

function deleteRecord(id) {
  const confirmed = window.confirm('Are you sure you want to delete this record?')
  if (!confirmed) return
  records.value = records.value.filter(r => r.id !== id)
  saveRecords()
  showFeedback('✕ Book record deleted.', 'error')
}

function startEdit(record) {
  editingId.value = record.id
  showForm.value = true
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

function cancelForm() {
  editingId.value = null
  showForm.value = false
}

function focusSearch() {
  searchHighlight.value = true
  setTimeout(() => searchHighlight.value = false, 600)
}

const filteredRecords = computed(() => {
  const keyword = searchTerm.value.toLowerCase().trim()
  if (!keyword) return records.value
  return records.value.filter(r =>
    r.title.toLowerCase().includes(keyword) ||
    r.author.toLowerCase().includes(keyword) ||
    r.category.toLowerCase().includes(keyword)
  )
})

const editingRecord = computed(() =>
  records.value.find(r => r.id === editingId.value) || null
)

const totalBooks = computed(() => records.value.length)
const availableCount = computed(() => records.value.filter(r => r.status === 'Available').length)
const borrowedCount = computed(() => records.value.filter(r => r.status === 'Borrowed').length)
</script>

<template>
  <div class="flex min-h-screen flex-col transition-all duration-500"
    :style="darkMode
      ? 'background: linear-gradient(135deg, #0a0a0a 0%, #111827 35%, #1a1a2e 65%, #0a0a0a 100%);'
      : 'background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 50%, #e2e8f0 100%);'">

    <AppHeader :darkMode="darkMode" @toggleTheme="toggleTheme" />

    <main class="mx-auto w-full max-w-6xl flex-1 px-4 py-8">

      <!-- Summary Cards -->
      <div class="mb-8 grid grid-cols-3 gap-4">
        <div v-for="(card, i) in [
          { label: 'Total Books', value: totalBooks, color: darkMode ? '#ffffff' : '#1e293b' },
          { label: 'Available', value: availableCount, color: darkMode ? '#86efac' : '#16a34a' },
          { label: 'Borrowed', value: borrowedCount, color: darkMode ? '#fcd34d' : '#d97706' }
        ]" :key="i"
          class="rounded-2xl border p-5 text-center transition-all duration-300 hover:scale-105 cursor-default"
          :style="darkMode
            ? 'background: linear-gradient(135deg, rgba(255,255,255,0.07) 0%, rgba(255,255,255,0.02) 100%); border-color: rgba(255,255,255,0.1); backdrop-filter: blur(10px);'
            : 'background: white; border-color: #e2e8f0; box-shadow: 0 1px 6px rgba(0,0,0,0.06);'">
          <p class="text-4xl font-bold transition-all duration-300" :style="`color: ${card.color};`">{{ card.value }}</p>
          <p class="mt-1 text-xs font-semibold uppercase tracking-widest"
            :style="darkMode ? 'color: rgba(255,255,255,0.35);' : 'color: #94a3b8;'">
            {{ card.label }}
          </p>
        </div>
      </div>

      <!-- Feedback Banner -->
      <transition name="banner">
        <div v-if="feedback.text"
          class="mb-5 rounded-xl border px-4 py-3 text-sm font-medium"
          :style="feedback.type === 'error'
            ? 'background: linear-gradient(90deg, rgba(239,68,68,0.12), rgba(239,68,68,0.04)); border-color: rgba(239,68,68,0.35); color: #fca5a5;'
            : darkMode
              ? 'background: linear-gradient(90deg, rgba(255,255,255,0.08), rgba(255,255,255,0.02)); border-color: rgba(255,255,255,0.15); color: #d1fae5;'
              : 'background: linear-gradient(90deg, #f0fdf4, #dcfce7); border-color: #86efac; color: #166534;'">
          {{ feedback.text }}
        </div>
      </transition>

      <!-- Controls -->
      <div class="mb-5 flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between">
        <!-- Search -->
        <div class="relative w-full sm:max-w-sm">
          <svg class="absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 transition-colors duration-200"
            :class="searchTerm ? (darkMode ? 'text-white' : 'text-slate-700') : 'text-gray-500'"
            fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-4.35-4.35M17 11A6 6 0 1 1 5 11a6 6 0 0 1 12 0z"/>
          </svg>
          <input
            v-model="searchTerm"
            @focus="focusSearch"
            type="text"
            placeholder="Search by title, author, or category..."
            class="w-full rounded-xl border pl-9 pr-4 py-2.5 text-sm focus:outline-none focus:ring-2 transition-all duration-300"
            :class="searchHighlight ? 'scale-105' : ''"
            :style="darkMode
              ? 'background: rgba(255,255,255,0.06); border-color: rgba(255,255,255,0.12); color: #e2e8f0; --tw-ring-color: rgba(255,255,255,0.2);'
              : 'background: white; border-color: #cbd5e1; color: #1e293b;'"
          />
          <!-- Clear search -->
          <button v-if="searchTerm" @click="searchTerm = ''"
            class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-white transition-colors duration-200 text-lg leading-none">
            ×
          </button>
        </div>

        <!-- Add Button -->
        <button v-if="!showForm"
          @click="showForm = true; editingId = null"
          class="flex items-center gap-2 rounded-xl px-6 py-2.5 text-sm font-bold shadow-lg transition-all duration-200 hover:scale-105 active:scale-95"
          :style="darkMode
            ? 'background: linear-gradient(135deg, #ffffff 0%, #d1d5db 100%); color: #000;'
            : 'background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); color: #fff;'">
          <span class="text-base leading-none">＋</span> Add Book
        </button>
      </div>

      <!-- Form with animation -->
      <transition name="form-slide">
        <div v-if="showForm" class="mb-6">
          <BookForm
            :editing="editingRecord"
            :darkMode="darkMode"
            @save="editingRecord ? updateRecord($event) : addRecord($event)"
            @cancel="cancelForm"
          />
        </div>
      </transition>

      <!-- Book List -->
      <BookList
        :books="filteredRecords"
        :search="searchTerm"
        :darkMode="darkMode"
        @edit="startEdit"
        @remove="deleteRecord"
      />
    </main>

    <AppFooter :darkMode="darkMode" />
  </div>
</template>

<style>
.banner-enter-active { transition: all 0.4s cubic-bezier(0.4,0,0.2,1); }
.banner-leave-active { transition: all 0.3s cubic-bezier(0.4,0,0.2,1); }
.banner-enter-from { opacity: 0; transform: translateY(-12px) scale(0.97); }
.banner-leave-to   { opacity: 0; transform: translateY(-8px) scale(0.97); }

.form-slide-enter-active { transition: all 0.35s cubic-bezier(0.4,0,0.2,1); }
.form-slide-leave-active { transition: all 0.25s cubic-bezier(0.4,0,0.2,1); }
.form-slide-enter-from { opacity: 0; transform: translateY(-16px); }
.form-slide-leave-to   { opacity: 0; transform: translateY(-8px); }
</style>
