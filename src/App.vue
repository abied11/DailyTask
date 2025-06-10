// File: src/App.vue
<template>
  <div :class="[isDark ? 'bg-gray-900 text-white' : 'bg-gradient-to-br from-purple-200 to-pink-100 text-black', 'min-h-screen p-6']">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold">🌿 Wishlist Healing 🌼</h1>
      <button @click="toggleTheme" class="px-4 py-2 rounded border shadow text-sm" :class="isDark ? 'bg-white text-black' : 'bg-black text-white'">
        {{ isDark ? 'Mode Terang' : 'Mode Gelap' }}
      </button>
    </div>

    <form @submit.prevent="addItem" class="flex gap-2 justify-center mb-6">
      <input
        v-model="newItem"
        type="text"
        placeholder="Mau healing ke mana?"
        class="px-4 py-2 rounded border shadow text-black"
      />
      <button
        type="submit"
        class="bg-purple-500 text-white px-4 py-2 rounded hover:bg-purple-600"
      >Tambah</button>
    </form>

    <div v-if="wishlist.length" class="grid gap-4 max-w-xl mx-auto">
      <div
        v-for="(item, index) in wishlist"
        :key="index"
        class="flex items-center justify-between bg-white p-4 rounded shadow"
        :class="[{ 'opacity-50': item.done }, isDark ? 'bg-gray-800 text-white' : 'bg-white text-black']"
      >
        <span :class="{ 'line-through': item.done }">{{ item.name }}</span>
        <div class="flex gap-2">
          <button
            @click="toggleDone(index)"
            class="text-sm px-2 py-1 rounded border border-green-500 text-green-600 hover:bg-green-100"
          >
            {{ item.done ? 'Belum' : 'Selesai' }}
          </button>
          <button
            @click="removeItem(index)"
            class="text-sm px-2 py-1 rounded border border-red-500 text-red-600 hover:bg-red-100"
          >
            Hapus
          </button>
        </div>
      </div>
    </div>

    <p v-else class="text-center text-gray-500">Wishlist healing kamu masih kosong 😢</p>
    <p v-if="allDone" class="text-center text-green-600 mt-4 font-semibold">Semua wishlist sudah terpenuhi! 🌈</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newItem = ref('')
const wishlist = ref([])
const isDark = ref(false)

function toggleTheme() {
  isDark.value = !isDark.value
}

function addItem() {
  if (newItem.value.trim() !== '') {
    wishlist.value.push({ name: newItem.value, done: false })
    newItem.value = ''
  }
}

function toggleDone(index) {
  wishlist.value[index].done = !wishlist.value[index].done
}

function removeItem(index) {
  wishlist.value.splice(index, 1)
}

const allDone = computed(() => wishlist.value.length > 0 && wishlist.value.every(item => item.done))
</script>

<style>
body {
  font-family: 'Segoe UI', sans-serif;
}
</style>
