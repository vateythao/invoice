<template>
  <div class="container my-4">
    <h1 class="text-center mb-0" style="font-family: impact">Vue Invoice</h1>
    <h4 class="text-center mt-0">#313234</h4>
    <div class="row">
      <div class="col-6">
        <strong class="d-block">Bill To:</strong>
        <span class="d-block">JOJO</span>
        <span class="d-block"> Customer ID:00123455</span>
        <span class="d-block"> Phone: 0123456789</span>
      </div>
      <div class="col-6 text-end">
        <strong class="d-block">Address</strong>
        <span class="d-block">St.Two Lips, North Korea,</span>
        <span class="d-block">Phnom Penh, Cambodia</span>
        <span class="d-block">12.12.2023</span>
      </div>
    </div>
    <table class="table table-bordered table-striped mt-3">
      <thead>
        <tr>
          <th>Name</th>
          <th width="100" class="text-end">Quantity</th>
          <th width="100" class="text-end">Price</th>
          <th width="100" class="text-end">Total</th>
          <th width="100" class="text-end">
            Actions
            <button @click="addNewProduct" class="btn btn-primary btn-sm">Add</button>
          </th>
          <!-- Edit button column -->
          <!-- Delete button column -->
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in products" :key="index">
          <td class="align-middle">
            <template v-if="editedItem.index == index">
              <input type="text" v-model="editedItem.record.name" />
            </template>
            <template v-else>{{ item.name }} </template>
          </td>
          <td class="align-middle text-end">
            <template v-if="editedItem.index == index">
              <input type="number" v-model="editedItem.record.quantity" />
            </template>
            <template v-else>
              {{ item.quantity }}
            </template>
          </td>
          <td class="align-middle text-end">
            <template v-if="editedItem.index == index">
              <input type="number" v-model="editedItem.record.price" />
            </template>
            <template v-else> ${{ item.price }} </template>
          </td>
          <td class="align-middle text">
            <template v-if="editedItem.index == index">
              {{ getSubTotal(editedItem.record.quantity, editedItem.record.price) }}
            </template>
            <template v-else> ${{ item.total.toFixed(2) }} </template>
          </td>

          <td>
            <div style="display: flex">
              <button v-if="editedItem.index == index" @click="submitUpdate()" class="btn btn-primary btn-sm">
                Save
              </button>
              <button v-if="editedItem.index == index" @click="submitUpdate()" class="btn btn-primary btn-sm">
                Cancel
              </button>
              <button v-else @click="editItem(item, index)" class="btn btn-primary btn-sm">
                Edit
              </button>

              <!-- <button @click="deleteItem(index)" class="btn btn-danger btn-sm">Delete</button> -->
              <button @click="deleteItem(index)" class="btn btn-danger btn-sm">Delete</button>
            </div>
          </td>
        </tr>

        <EditProductForm v-if="isFormAddVisible" v-model:name="editedItem.record.name"
          v-model:quantity="editedItem.record.quantity" v-model:price="editedItem.record.price"
          @save="submitNewProduct" @close="closeInput()" />
        <tr>
          <td colspan="5">
            <!-- <hr /> -->
          </td>
        </tr>
        <tr>
          <td class="align-middle text-end" colspan="3">Sub-Total</td>
          <td class="align-middle text-end">${{ subtotal.toFixed(2) }}</td>
        </tr>
        <tr>
          <td class="align-middle text-end" colspan="3">Sales Tax({{ displayTax }}%)</td>
          <td class="align-middle text-end" v-effect="(taxtotal = Number(subtotal) * Number(tax))">
            ${{ taxtotal.toFixed(2) }}
          </td>
        </tr>
        <tr>
          <td class="align-middle text-end" colspan="3">Total Price</td>
          <td class="align-middle text-end">${{ pricetotal.toFixed(2) }}</td>
        </tr>
      </tbody>
    </table>
    <br />
    <br />
  </div>
</template>
<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'
import EditProductForm from './EditProductForm.vue'

 let showAddInput = ref(false) 


interface Product {
  name: string
  quantity: number
  price: number
  total: number
}

enum FormActions {
  ADD = 'add',
  EDIT = 'edit',
  CLOSE = 'close'
}

interface EditedItem {
  index?: number
  action?: FormActions
  record: Product
}

const products = ref<Product[]>([
  { name: 'Coca', quantity: 3, price: 2, total: 6 },
  { name: 'Sprite', quantity: 2, price: 2, total: 4 },
  { name: 'Fanta', quantity: 5, price: 2.5, total: 12.5 },
  { name: 'Pepsi', quantity: 5, price: 2, total: 10 }
])

const editedItem = ref<EditedItem>({
  index: undefined,
  action: undefined,
  record: { name: '', quantity: 0, price: 0, total: 0 }
})


const isFormAddVisible = computed(() => {
  return editedItem.value.action === FormActions.ADD
})

const subtotal = computed(() => {
  let total = 0
  products.value.forEach((item) => {
    total += item.total
  })
  return total

  //   return products.value.reduce((acc, item) => acc + item.total, 0)
})

const tax = ref<number>(0.06) // Set default value for tax
const displayTax = computed(() => {
  return tax.value * 100
})
const taxtotal = computed(() => {
  return subtotal.value * tax.value
})

const pricetotal = computed(() => {
  return subtotal.value + taxtotal.value
})

onMounted(() => {
  const savedItems = JSON.parse(localStorage.getItem('products') || '[]')

  if (savedItems.length > 0) {
    products.value = Object.assign([], savedItems)
  }
})

function getSubTotal(qty: number, price: number) {
  return (qty * price).toFixed(2)
}
// Load products from local storage or use default values
// const editItem = (index: number) => {
//   products[index].edit = true
// }

function editItem(product: any, index: number) {
  editedItem.value.index = index
  editedItem.value.record = Object.assign({}, product)
}

function submitUpdate() {
  const newItem: Product = {
    name: editedItem.value.record.name,
    quantity: editedItem.value.record.quantity,
    price: editedItem.value.record.price,
    total: editedItem.value.record.quantity * editedItem.value.record.price
  }
  if (editedItem.value.index !== undefined) {
    products.value.splice(editedItem.value.index, 1, newItem)
  }

  editedItem.value.index = undefined
  editedItem.value.record = { name: '', quantity: 0, price: 0, total: 0 }
}

function closeInput(){
  editedItem.value.action = FormActions.CLOSE
}
// const deleteItem = (index: number) => {
//   products.value.splice(index, 1)
// }
function deleteItem(index: number) {
  // Show alert before deleting the item
  const confirmDelete = window.confirm('Are you sure you want to delete this item?')
  
  if (confirmDelete) {
    products.value.splice(index, 1)
  }
}

function addNewProduct() {
  editedItem.value.action = FormActions.ADD
}

function submitNewProduct() {
  const newItem: Product = {
    name: editedItem.value.record.name,
    quantity: editedItem.value.record.quantity,
    price: editedItem.value.record.price,
    total: editedItem.value.record.quantity * editedItem.value.record.price
  }

  products.value.push(newItem)
  editedItem.value.action = undefined
  editedItem.value.record = { name: '', quantity: 0, price: 0, total: 0 }
}
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
}

h1 {
  font-size: 32px;
  font-weight: bold;
  margin-bottom: 10px;
}

h4 {
  font-size: 18px;
  margin-top: 0;
}

.row {
  margin-top: 20px;
}

.col-6 {
  width: 50%;
  float: left;
}

.text-end {
  text-align: end;
}

.table {
  width: 100%;
  margin-top: 20px;
  border-collapse: collapse;
}

.table th,
.table td {
  padding: 10px;
  border: 1px solid #ccc;
}

.table th {
  background-color: #f0f0f0;
  font-weight: bold;
}

.table-bordered {
  border: 1px solid #ccc;
}

.table-striped tbody tr:nth-of-type(odd) {
  background-color: #f9f9f9;
}

.text {
  text-align: center;
}

.btn {
  padding: 5px 10px;
  margin-right: 5px;
  border-radius: 3px;
  border: none;
  color: #fff;
  cursor: pointer;
}

.btn-primary {
  background-color: #007bff;
}

.btn-danger {
  background-color: #dc3545;
}

input[type="text"],
input[type="number"] {
  width: 100%;
  padding: 5px;
}

hr {
  border: none;
  border-top: 1px solid #ccc;
  margin: 20px 0;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 5px;
}

h1 {
  font-size: 32px;
  font-family: Impact, sans-serif;
}

h4 {
  font-size: 18px;
  font-weight: normal;
  color: #888;
}

.row {
  margin-top: 20px;
}

.col-6 {
  width: 50%;
}

strong {
  font-weight: bold;
}

.d-block {
  display: block;
  margin-bottom: 5px;
}

.text-end {
  text-align: end;
}

.text-center {
  text-align: center;
}

.my-4 {
  margin-top: 40px;
  margin-bottom: 40px;
}
</style>
