<!-- <template>
    
  <div class="container my-4">
    <h1 class="text-center mb-0" style="font-family: impact;">Vue Invoice</h1>
    <h4 class="text-center mt-0">#313234</h4>
    <div class="row">
        <div class="col-6">
            <strong class="d-block">Bill To</strong>
            <span class="d-block">Vatey</span>
            <span class="d-block"> Customer ID: 00123455</span>
            <span class="d-block"> Phone: 0123456789</span>
        </div>
        <div class="col-6 text-end">
            <strong class="d-block">Address</strong>
            <span class="d-block">St. Two Lips, North Korea,</span>
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
            </tr>
        </thead>
        <tbody>
            <tr v-for="(item, index) in items" :key="index">
                <td class="align-middle">{{ item.name}}</td>
                <td class="align-middle text-end">{{ item.quantity}}</td>
                <td class="align-middle text-end">${{ item.price}}</td>
                <td class="align-middle text-end" v-effect="item.total = Number(item.quantity) * Number(item.price)">${{item.total.toFixed(2)}}</td>
            </tr>
            <tr>
                <td class="align-middle text-end" colspan="3">Sub-Total</td>
                <td class="align-middle text-end">
                    ${{  subtotal.toFixed(2) }}
                </td>
            </tr>
            <tr>
                <td class="align-middle text-end" colspan="3">Sales Tax(6%)</td>
                <td class="align-middle text-end" v-effect="taxtotal = Number(subtotal) * Number(tax)">
                    ${{ taxtotal.toFixed(2) }}
                </td>
            </tr>
            <tr>
                <td class="align-middle text-end" colspan="3">Total Price</td>
                <td class="align-middle text-end">
                    ${{ pricetotal.toFixed(2) }}
                </td>
            </tr>
        </tbody>
       
    </table>
</div>
  
</template> -->

<!-- <script setup lang="ts">
import {ref, reactive, watchEffect} from 'vue'


const items = reactive<Array<{name:string, quantity:number, price:number, total:number,edit: boolean}>>([
    {name: 'Product Item 1', quantity: 3, price: 12, total: 0, edit: false},
    {name: 'Product Item 1', quantity: 3, price: 12, total: 0, edit: false},
    {name: 'Product Item 1', quantity: 3, price: 12, total: 0, edit: false},
    {name: 'Product Item 1', quantity: 3, price: 12, total: 0, edit: false}
    
])
const subtotal= ref<number>(0)
const tax = ref<number>(0)
const taxtotal = ref<number>(0)
const pricetotal = ref<number>(0)


tax.value = 0.86

watchEffect(() => {
    items.map(item => {
        item.total = Number(item.quantity) * Number(item.price)
        return item 
    })
    subtotal.value = items.length >=1 ? items.map(i => Number(i.total)).reduce((acc, val) => acc + val) : 0
    taxtotal.value = Number(subtotal.value) * Number(tax.value)
    pricetotal.value = Number(subtotal.value) + Number(taxtotal.value)

})
defineExpose({
    items,
    subtotal,
    tax,
    taxtotal,
    pricetotal

})


</script>
<style scoped>
.container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    background-color: #050404;
    border: 1px solid #d31313;
    font-family: Arial, sans-serif;
  }
  
  h1 {
    font-size: 28px;
    font-weight: bold;
    text-align: center;
    margin-bottom: 0;
  }
  
  h4 {
    font-size: 18px;
    font-weight: normal;
    text-align: center;
    margin-top: 0;
    margin-bottom: 20px;
  }
  
  .row {
    display: flex;
    justify-content: space-between;
  }
  
  .col-6 {
    width: 50%;
  }
  
  .d-block {
    display: block;
    margin-bottom: 5px;
  }
  
  .table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }
  
  .table th,
  .table td {
    padding: 10px;
    border: 1px solid #000000;
  }
  
  .table th {
    background-color: #000000;
  }
  
  .text-end {
    text-align: right;
  }
  
  .align-middle {
    vertical-align: middle;
  }
  
  .text-center {
    text-align: center;
  }
  
  .mt-3 {
    margin-top: 15px;
  }
  
  .my-4 {
    margin-top: 40px;
    margin-bottom: 40px;
  }
  
</style> -->

  
    

    

