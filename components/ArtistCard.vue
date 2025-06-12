<script setup lang="ts">
defineProps<{
  artist: {
    name: string;
    description: string;
    artist_id: string;
  };
}>();
</script>
<template>
  <v-card :elevation="0">
    <v-card-item>
      <v-card-title>
        <NuxtLink to="/artist">{{ artist.name }}</NuxtLink>
      </v-card-title>
      <v-card-subtitle>
        {{ artist.description }}
      </v-card-subtitle>
    </v-card-item>
  </v-card>
</template>
