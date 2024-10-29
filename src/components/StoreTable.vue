<template>
  <v-container>
    <v-data-table
      :headers="headers"
      :items="products"
      :search="search"
      class="elevation-1"
    >
      <!-- Search Bar and Register Button in Same Row -->
      <template v-slot:top>
        <v-row>
          <!-- Search Bar on the Left -->
          <v-col>
            <v-text-field
              v-model="search"
              label="ค้นหาสินค้าในคลัง"
              clearable
            />
          </v-col>
          
          <!-- Spacer to push the button to the right -->
          <v-spacer></v-spacer>
          
          <!-- Register Button on the Right -->
          <v-col class="d-flex justify-end">
            <v-btn color="primary" @click="goToRegister">
              ไปหน้าลงทะเบียนสินค้า
            </v-btn>
          </v-col>
        </v-row>
      </template>

      <!-- Table Rows with only 'Dispense' Action -->
      <template v-slot:item.actions="{ item }">
        <v-btn color="red" @click="dispenseProduct(item)" class="mx-2">
          จ่ายสินค้า
        </v-btn>
      </template>
    </v-data-table>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

const router = useRouter()
const search = ref('')
const products = ref([])

// Table headers
const headers = ref([
  { title: 'รหัสสินค้า', value: 'id' },
  { title: 'รายละเอียดสินค้า', value: 'name' },
  { title: 'รายละเอียดประเภทสินค้า', value: 'type' },
  { title: 'ตำแหน่งสินค้า', value: 'position' },
  { title: 'วันที่จัดเก็บสินค้า', value: 'date' },
  { title: 'Actions', value: 'actions', sortable: false },
])

// Function to format datetime, return 'None' if datetime is null
const formatDateTime = (datetime) => {
  if (!datetime) return 'None'
  const date = new Date(datetime)
  return date.toLocaleString('th-TH', {
    year: 'numeric',
    month: '2-digit',
    day: '2-digit',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
  })
}

// Fetch data from API on component mount
const fetchProducts = async () => {
  try {
    const response = await axios.get('http://10.80.86.7:8000/zones/')
    products.value = response.data.zones
      .filter(zone => zone.status === 'occupied')  // Filter for only "occupied" zones
      .map(zone => ({
        id: zone.productCode,
        name: zone.additionalInfo,
        type: zone.productType,
        position: zone.name,
        date: formatDateTime(zone.datetime),
      }))
  } catch (error) {
    console.error('Error fetching zones data:', error)
  }
}

onMounted(fetchProducts)  // Call the fetchProducts function on component mount

// Method for the 'Dispense' action button
const dispenseProduct = async (item) => {
  try {
    const response = await axios.post('http://10.80.86.7:8000/pickup-from-store/', null, {
      params: { zone_id: item.position },  // Ensure this matches the parameter name in FastAPI
    });
    console.log('Product dispensed successfully:', response.data);
    
    // Refresh products data after dispensing
    await fetchProducts();
    
  } catch (error) {
    console.error('Error dispensing product:', error);
  }
};

const goToRegister = () => {
  router.push('/RegisterPackage')
}
</script>
