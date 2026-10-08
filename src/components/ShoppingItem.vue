<script setup>
import { ref } from 'vue'

const props = defineProps({ item: Object })
const emit = defineEmits(['delete', 'update'])

const isEditing = ref(false)
const editedName = ref(props.item.name)
const editedQuantity = ref(props.item.quantity)

function updateItem() {
  if (editedName.value.trim() === '' || editedQuantity.value < 1) return
  emit('update', {
  ...props.item,
    name: editedName.value,
    quantity: Number(editedQuantity.value)
  })
  isEditing.value = false
}

function toggleBought() {
  emit('update', {...props.item, bought:!props.item.bought })
}

function deleteItem() {
  emit('delete', props.item.id)
}

function startEditing() {
  editedName.value = props.item.name
  editedQuantity.value = props.item.quantity
  isEditing.value = true
}
</script>

<template>
  <div class="shopping-item">
    <!-- VIEW MODE -->
    <div v-if="!isEditing" class="view-mode">
      <input type="checkbox" :checked="item.bought" @change="toggleBought" />
      <span :class="{ bought: item.bought }">
        {{ item.name }} - Qty: {{ item.quantity }}
      </span>
      <button @click="startEditing">Edit</button>
      <button @click="deleteItem">Delete</button>
    </div>

    <!-- EDIT MODE -->
    <div v-else class="edit-mode">
      <input v-model="editedName" placeholder="Item Name" />
      <input v-model.number="editedQuantity" type="number" min="1" />
      <button @click="updateItem">Save</button>
      <button @click="isEditing = false">Cancel</button>
    </div>
  </div>
</template>