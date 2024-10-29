<template>
  <v-app>
    <!-- App Bar (Navigation) -->
    <v-app-bar app>
      <v-icon left class="ml-6">
        mdi-robot-industrial-outline
      </v-icon>

      <!-- Content of the app bar, e.g., a title -->
      <v-toolbar-title> Smart Stroage </v-toolbar-title>
    </v-app-bar>

    <!-- Main Content -->
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup>
// Any JavaScript logic or setup can go here
</script>
