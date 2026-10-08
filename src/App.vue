<script setup>
import { ref } from 'vue'
import ShoppingItem from './components/ShoppingItem.vue'

const items = ref([
  { id: 1, name: 'Milk', quantity: 2, bought: false },
  { id: 2, name: 'Bread', quantity: 1, bought: true },
])

const newName = ref('')
const newQuantity = ref(1)
let nextId = 3

// CREATE
function addItem() {
  if (newName.value.trim() === '' || newQuantity.value < 1) {
    alert('Please enter item name and valid quantity')
    return
  }
  items.value.push({
    id: nextId++,
    name: newName.value,
    quantity: Number(newQuantity.value),
    bought: false
  })
  newName.value = ''
  newQuantity.value = 1
}

// UPDATE
function updateItem(updatedItem) {
  const index = items.value.findIndex(i => i.id === updatedItem.id)
  if (index!== -1) {
    items.value[index] = updatedItem
  }
}

// DELETE
function deleteItem(id) {
  items.value = items.value.filter(i => i.id!== id)
}
</script>

<template>
  <div class="container">
    <h1>Shopping List</h1>

    <!-- CREATE FORM -->
    <div class="add-item">
      <input v-model="newName" placeholder="Item Name" @keyup.enter="addItem" />
      <input v-model.number="newQuantity" type="number" min="1" />
      <button @click="addItem">Add</button>
    </div>

    <!-- LIST -->
    <div v-if="items.length === 0" class="empty">Your shopping list is empty</div>

    <ShoppingItem
      v-for="item in items"
      :key="item.id"
      :item="item"
      @update="updateItem"
      @delete="deleteItem"
    />
  </div>
</template>