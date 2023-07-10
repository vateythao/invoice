<template>
  <tr>
    <td>
      <input type="text" placeholder="name" :value="name" @input="onInputName" />
    </td>
    <td>
      <input type="number" placeholder="Quantity" :value="quantity" @input="onInputQty" />
    </td>
    <td>
      <input type="number" placeholder="Price" :value="price" @input="onInputPrice" />
    </td>
    <td>
      {{ subtotal }}
    </td>
    <td  style="display: flex">
      <button @click="submitUpdate()" class="btn btn-primary btn-sm">Save</button>
      <button @click="closeUpdate()" class="btn btn-primary btn-sm">Cancel</button>
    </td>
  </tr>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps({
  name: String,
  quantity: { type: Number, default: 0 },
  price: { type: Number, default: 0 }
})

const subtotal = computed(() => {
  return '$' + (props.quantity * props.price).toFixed(2)
  //   return `$${(props.quantity * props.price).toFixed(2)}`
})

const emit = defineEmits(['update:name', 'update:quantity', 'update:price', 'save'])

function onInputName(event: any) {
  emit('update:name', event.target.value)
}
function onInputQty(event: any) {
  emit('update:quantity', event.target.value)
}
function onInputPrice(event: any) {
  emit('update:price', event.target.value)
}

function submitUpdate() {
  emit('save')
}

function closeUpdate(){
  emit('close')
}

</script>
