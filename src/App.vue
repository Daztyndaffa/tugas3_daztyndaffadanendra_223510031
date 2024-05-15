<!--
https://eugenkiss.github.io/7guis/tasks/#crud
-->

<script setup>
import { ref, reactive, computed, watch } from 'vue'

const students = reactive(['223510001, Daztyn, B', '223510002, Daffa, A', '223510003, Danendra, C'])
const selected = ref('')
const prefix = ref('')
const npm = ref('')
const nama = ref('')
const kelas = ref('')


const filteredNames = computed(() =>
  students.filter((n) =>
    n.toLowerCase().startsWith(prefix.value.toLowerCase())
  )
)

watch(selected, (name) => {
  ;[npm.value, nama.value, kelas.value] = name.split(', ')
})

function create() {
  if (hasValidInput()) {
    const Student = `${npm.value}, ${nama.value}, ${kelas.value}`
    if (!students.includes(Student)) {
      students.push(Student)
      npm.value = nama.value = kelas.value =''
    }
  }
}

function update() {
  if (hasValidInput() && selected.value) {
    const i = students.indexOf(selected.value)
    students[i] = selected.value = `${npm.value}, ${nama.value}, ${kelas.value}`
  }
}

function del() {
  if (selected.value) {
    const i = students.indexOf(selected.value)
    students.splice(i, 1)
    selected.value = npm.value = nama.value = kelas.value= ''
  }
}

function hasValidInput() {
  return npm.value.trim() && nama.value.trim() && kelas.value.trim()
}
</script>

<template>
  <div class="container">
    <header>
      <div class="logo-container">
        <a href="https://vitejs.dev" target="_blank">
          <img src="/vite.svg" class="logo" alt="Vite logo" />
        </a>
        <a href="https://vuejs.org/" target="_blank">
          <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
        </a>
      </div>
    </header>

    <div class="filter-container">
      <input v-model="prefix" placeholder="Filter prefix" />
    </div>

    <div class="selection-container">
      <select size="5" v-model="selected">
        <option v-for="name in filteredNames" :key="name">{{ name }}</option>
      </select>
    </div>

    <div class="form-container">
      <label>NPM: <input v-model="npm"></label>
    </div>

    <div class="form-container">
      <label>Nama: <input v-model="nama"></label>
    </div>

    <div class="form-container">
      <label>Kelas: <input v-model="kelas"></label>
    </div>

    <div class="buttons">
      <button @click="create">Create</button>
      <button @click="update">Update</button>
      <button @click="del">Delete</button>
    </div>
  </div>
</template>

<style>
.container {
  max-width: 600px;
  margin: 0 auto;
}

.logo-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.filter-container {
  margin-bottom: 20px;
}

.selection-container {
  margin-bottom: 20px;
}

.form-container {
  margin-bottom: 20px;
}

.buttons {
  display: flex;
}

.buttons button + button {
  margin-left: 10px;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

h1 {
  color: #646cff;
  margin-bottom: 50px;
}

span {
  color: #42b883;
}

.done {
  text-decoration: line-through;
}
</style>
