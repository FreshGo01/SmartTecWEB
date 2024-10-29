<template>
  <v-container>
    <!-- Title at the top-left -->
    <v-row justify="center">
      <h1 class="text-h4 underline">Store</h1>
    </v-row>

    <!-- Card containing StoreTable -->
    <v-row>
      <v-col>
        <v-card class="mx-auto my-5" max-width="1600" elevation="6">
          <v-card-text>
            <StoreTable />
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
// Any JavaScript logic or setup can go here
</script>
<style scoped>
.underline {
  text-decoration: underline;
}
</style>