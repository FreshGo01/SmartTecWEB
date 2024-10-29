<template>
  <v-container>
    <!-- Centered Page Title with Underline -->
    <v-row justify="center">
      <h1 class="text-h4 underline">ลงทะเบียนสินค้า</h1>
    </v-row>

    <!-- Single Card containing Input Fields -->
    <v-row justify="center">
      <v-col cols="12" md="8">
        <v-card class="mx-auto" max-width="800" elevation="6">
          <v-card-text>
            <v-row>
              <!-- Right Column: Form Inputs -->
              <v-col cols="12" md="12">
                <v-text-field
                  v-model="additionalInfo"
                  label="รายละเอียดสินค้า"
                  clearable
                />

                <v-select
                  v-model="productType"
                  :items="productTypes"
                  label="ประเภทสินค้า"
                />

                <v-select
                  v-model="storagePosition"
                  :items="availableZones"
                  label="ตำแหน่งในการจัดเก็บ"
                />

                <!-- <v-btn color="primary" class="mx-2" @click="scanBarcode">สแกนบาร์โค้ด</v-btn> -->
                <v-btn color="primary" class="mx-2" @click="submit">บันทึก</v-btn>
                <v-btn color="red" class="mx-2" text @click="() => $router.go(-1)">กลับ</v-btn>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();

// Dummy data for product types
const productTypes = ref(["ประเภท A", "ประเภท B", "ประเภท C"]);
const availableZones = ref([]); // Store available zones

const additionalInfo = ref("");
// const productCode = ref(""); // Captured from the barcode
const productType = ref("");
const storagePosition = ref("");

// Fetch available zones from the API
onMounted(async () => {
  try {
    const response = await axios.get("http://10.80.86.7:8000/available-zones/");
    availableZones.value = response.data.available_zones; // Set available zones
  } catch (error) {
    console.error("Error fetching available zones:", error);
  }
});

// Simulate barcode scanning and populate productCode
// const scanBarcode = () => {
//   productCode.value = "123456789"; // Replace with actual scanned data
//   alert(`Barcode captured: ${productCode.value}`);
// };

// Submit handler to call the FastAPI /storage endpoint
const submit = async () => {
    try {
        const response = await axios.post("http://10.80.86.7:8000/storage/", {
            zone_id: storagePosition.value,
            productType: productType.value,
            additionalInfo: additionalInfo.value
        });
        console.log("Success:", response.data);
        router.push("/"); // Redirect to home page
    } catch (error) {
        console.error("Error:", error.response?.data || error.message);
    }
};
</script>

<style>
/* Style for underline text */
.underline {
  text-decoration: underline;
}
</style>
