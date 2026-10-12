<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const users = ref([]);
const loading = ref(true);
const error = ref(null);

// Ganti URL ini sesuai URL API Laravel Vercel milikmu
const API_URL =
  "https://api-vercel-f4fwj3ncg-salsabilablokc-2523s-projects.vercel.app/api/api/users";

const fetchUsers = async () => {
  try {
    const response = await axios.get(API_URL);
    // Mengambil array data dari respon API Laravel
    users.value = response.data.data;
  } catch (err) {
    error.value = "Gagal mengambil data dari API: " + err.message;
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchUsers();
});
</script>

<template>
  <div style="padding: 20px; font-family: sans-serif">
    <h1>Daftar Pengguna (Vue + Laravel API)</h1>

    <p v-if="loading">Sedang memuat data...</p>
    <p v-else-if="error" style="color: red">{{ error }}</p>

    <ul v-else>
      <li v-for="user in users" :key="user.id">
        <strong>{{ user.name }}</strong> - {{ user.email }}
      </li>
    </ul>
  </div>
</template>
