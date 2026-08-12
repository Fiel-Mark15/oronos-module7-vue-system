<script setup>
import { reactive, watch } from 'vue'

const props = defineProps({ editing: Object, darkMode: Boolean })
const emit = defineEmits(['save', 'cancel'])

const CATEGORIES = ['Science','Technology','Engineering','Mathematics','Literature','History','Reference','Other']

const form = reactive({ title:'', author:'', category:'', status:'Available', borrowedBy:'' })
const errors = reactive({ title:'', author:'', category:'', borrowedBy:'' })

watch(() => props.editing, (val) => {
  if (val) {
    form.title = val.title; form.author = val.author
    form.category = val.category; form.status = val.status
    form.borrowedBy = val.borrowedBy || ''
  } else {
    Object.assign(form, { title:'', author:'', category:'', status:'Available', borrowedBy:'' })
  }
}, { immediate: true })

function validate() {
  errors.title    = form.title.trim()    ? '' : 'Book title is required.'
  errors.author   = form.author.trim()   ? '' : 'Author name is required.'
  errors.category = form.category        ? '' : 'Please select a category.'
  errors.borrowedBy = (form.status === 'Borrowed' && !form.borrowedBy.trim()) ? "Enter the borrower's name." : ''
  return !errors.title && !errors.author && !errors.category && !errors.borrowedBy
}

function submit() {
  if (!validate()) return
  const payload = { ...form }
  if (form.status === 'Available') payload.borrowedBy = ''
  if (props.editing) payload.id = props.editing.id
  emit('save', payload)
  Object.assign(form, { title:'', author:'', category:'', status:'Available', borrowedBy:'' })
  Object.assign(errors, { title:'', author:'', category:'', borrowedBy:'' })
}

const inputStyle = (hasError) => {
  const base = 'width:100%;border-radius:0.75rem;padding:0.625rem 0.75rem;font-size:0.875rem;transition:all 0.2s;outline:none;'
  if (hasError) return base + 'background:rgba(239,68,68,0.08);border:1px solid rgba(239,68,68,0.5);color:#e2e8f0;'
  return base + 'background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.12);color:#e2e8f0;'
}
const lightInputStyle = (hasError) => {
  const base = 'width:100%;border-radius:0.75rem;padding:0.625rem 0.75rem;font-size:0.875rem;transition:all 0.2s;outline:none;'
  if (hasError) return base + 'background:#fef2f2;border:1px solid #fca5a5;color:#1e293b;'
  return base + 'background:#f8fafc;border:1px solid #cbd5e1;color:#1e293b;'
}
</script>

<template>
  <div class="rounded-2xl border p-6 transition-all duration-500"
    :style="darkMode
      ? 'background:linear-gradient(135deg,rgba(255,255,255,0.06) 0%,rgba(255,255,255,0.02) 100%);border-color:rgba(255,255,255,0.1);backdrop-filter:blur(12px);'
      : 'background:white;border-color:#e2e8f0;box-shadow:0 4px 24px rgba(0,0,0,0.08);'">

    <!-- Header -->
    <div class="mb-5 flex items-center justify-between">
      <h2 class="text-base font-bold" :class="darkMode ? 'text-white' : 'text-slate-800'">
        {{ editing ? '✏️ Edit Book Record' : '📖 Add New Book' }}
      </h2>
      <span class="rounded-full px-3 py-0.5 text-xs font-semibold"
        :style="darkMode
          ? 'background:rgba(255,255,255,0.08);color:rgba(255,255,255,0.6);border:1px solid rgba(255,255,255,0.12);'
          : 'background:#f1f5f9;color:#475569;border:1px solid #cbd5e1;'">
        {{ editing ? 'Edit Mode' : 'Add Mode' }}
      </span>
    </div>

    <div class="grid gap-4 sm:grid-cols-2">
      <!-- Title -->
      <div class="sm:col-span-2">
        <label class="mb-1 block text-sm font-medium" :class="darkMode ? 'text-gray-300' : 'text-slate-700'">
          Book Title <span class="text-red-400">*</span>
        </label>
        <input v-model="form.title" type="text" placeholder="e.g. Introduction to Computer Science"
          :style="darkMode ? inputStyle(errors.title) : lightInputStyle(errors.title)"
          :class="darkMode ? 'placeholder-gray-600 focus:ring-2 focus:ring-white/20' : 'placeholder-slate-300'" />
        <transition name="err"><p v-if="errors.title" class="mt-1 text-xs text-red-400">{{ errors.title }}</p></transition>
      </div>

      <!-- Author -->
      <div>
        <label class="mb-1 block text-sm font-medium" :class="darkMode ? 'text-gray-300' : 'text-slate-700'">
          Author <span class="text-red-400">*</span>
        </label>
        <input v-model="form.author" type="text" placeholder="e.g. John Santos"
          :style="darkMode ? inputStyle(errors.author) : lightInputStyle(errors.author)" />
        <transition name="err"><p v-if="errors.author" class="mt-1 text-xs text-red-400">{{ errors.author }}</p></transition>
      </div>

      <!-- Category -->
      <div>
        <label class="mb-1 block text-sm font-medium" :class="darkMode ? 'text-gray-300' : 'text-slate-700'">
          Category <span class="text-red-400">*</span>
        </label>
        <select v-model="form.category"
          :style="darkMode ? inputStyle(errors.category) : lightInputStyle(errors.category)">
          <option value="" disabled :style="darkMode ? 'background:#1f2937;' : ''">Select a category</option>
          <option v-for="cat in CATEGORIES" :key="cat" :style="darkMode ? 'background:#1f2937;color:#e5e7eb;' : ''">{{ cat }}</option>
        </select>
        <transition name="err"><p v-if="errors.category" class="mt-1 text-xs text-red-400">{{ errors.category }}</p></transition>
      </div>

      <!-- Status -->
      <div>
        <label class="mb-1 block text-sm font-medium" :class="darkMode ? 'text-gray-300' : 'text-slate-700'">Status</label>
        <select v-model="form.status"
          :style="darkMode
            ? 'width:100%;border-radius:0.75rem;padding:0.625rem 0.75rem;font-size:0.875rem;background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.12);color:#e2e8f0;outline:none;'
            : 'width:100%;border-radius:0.75rem;padding:0.625rem 0.75rem;font-size:0.875rem;background:#f8fafc;border:1px solid #cbd5e1;color:#1e293b;outline:none;'">
          <option :style="darkMode ? 'background:#1f2937;' : ''">Available</option>
          <option :style="darkMode ? 'background:#1f2937;' : ''">Borrowed</option>
        </select>
      </div>

      <!-- Borrowed By (conditional with animation) -->
      <transition name="field-pop">
        <div v-if="form.status === 'Borrowed'">
          <label class="mb-1 block text-sm font-medium" :class="darkMode ? 'text-gray-300' : 'text-slate-700'">
            Borrowed By <span class="text-red-400">*</span>
          </label>
          <input v-model="form.borrowedBy" type="text" placeholder="Student name"
            :style="darkMode ? inputStyle(errors.borrowedBy) : lightInputStyle(errors.borrowedBy)" />
          <transition name="err"><p v-if="errors.borrowedBy" class="mt-1 text-xs text-red-400">{{ errors.borrowedBy }}</p></transition>
        </div>
      </transition>
    </div>

    <!-- Buttons -->
    <div class="mt-6 flex flex-wrap gap-3">
      <button @click="submit"
        class="rounded-xl px-6 py-2.5 text-sm font-bold shadow-lg transition-all duration-200 hover:scale-105 active:scale-95"
        :style="darkMode
          ? 'background:linear-gradient(135deg,#ffffff 0%,#d1d5db 100%);color:#000;'
          : 'background:linear-gradient(135deg,#0f172a 0%,#1e293b 100%);color:#fff;'">
        {{ editing ? 'Save Changes' : 'Add Book' }}
      </button>
      <button @click="emit('cancel')"
        class="rounded-xl px-6 py-2.5 text-sm font-medium transition-all duration-200 hover:scale-105 active:scale-95"
        :style="darkMode
          ? 'border:1px solid rgba(255,255,255,0.15);color:rgba(255,255,255,0.55);background:transparent;'
          : 'border:1px solid #cbd5e1;color:#475569;background:#f8fafc;'">
        Cancel
      </button>
    </div>
  </div>
</template>

<style scoped>
.err-enter-active { transition: all 0.25s ease; }
.err-enter-from   { opacity: 0; transform: translateY(-4px); }
.field-pop-enter-active { transition: all 0.3s cubic-bezier(0.4,0,0.2,1); }
.field-pop-leave-active { transition: all 0.2s ease; }
.field-pop-enter-from { opacity: 0; transform: scale(0.95) translateY(-6px); }
.field-pop-leave-to   { opacity: 0; transform: scale(0.95); }
</style>
<- Validation added -->
