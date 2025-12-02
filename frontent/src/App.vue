<script setup>
import { ref, onMounted } from "vue";

const items = ref([]);
const newItem = ref("");
const backendUrl = import.meta.env.VITE_BACKEND_URL || "http://localhost:3000";

async function fetchItems() {
  const res = await fetch(`${backendUrl}/api/items`);
  items.value = await res.json();
}

async function addItem() {
  if (!newItem.value.trim()) return;

  await fetch(`${backendUrl}/api/items`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: newItem.value })
  });

  newItem.value = "";
  await fetchItems();
}

onMounted(fetchItems);
</script>

<template>
  <main style="max-width: 600px; margin: 2rem auto; font-family: sans-serif;">
    <h1>Simple Vue + Express + SQLite App</h1>

    <section style="margin: 1rem 0;">
      <h2>Add item</h2>
      <input
        v-model="newItem"
        placeholder="Item name"
        style="padding: 0.5rem; width: 60%;"
      />
      <button @click="addItem" style="padding: 0.5rem 1rem; margin-left: 0.5rem;">
        Add
      </button>
    </section>

    <section>
      <h2>Items</h2>
      <ul>
        <li v-for="item in items" :key="item.id">
          {{ item.name }}
        </li>
      </ul>
    </section>
  </main>
</template>
