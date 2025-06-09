<script setup lang="ts">
defineProps<{
  track: {
    title: string;
    year: string;
    artist_id: string;
    album_id: string;
  };
}>();
</script>
<template>
  <v-card :elevation="0">
    <v-card-item>
      <v-card-title> {{ track.title }} </v-card-title>

      <v-card-subtitle>
        <NuxtLink to="/artist">{{ track.artist_id }}</NuxtLink>
      </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      {{ track.year }}
      <NuxtLink to="/album">{{ track.album_id }}</NuxtLink>
    </v-card-text>
  </v-card>
</template>
