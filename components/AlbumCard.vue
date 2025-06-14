<script setup lang="ts">
defineProps<{
  album: {
    album_name: string;
    year: string;
    artist_name: string;
    genre_name: string;
  };
}>();
</script>

<template>
  <v-card :elevation="4">
    <v-card-item>
      <v-card-title class="text-h5 font-weight-black"
        ><NuxtLink to="/album">{{ album.album_name }}</NuxtLink></v-card-title
      >
      <v-card-subtitle class="text-h6 font-weight-black">
        <NuxtLink to="/artist">{{ album.artist_name }}</NuxtLink>
      </v-card-subtitle>
    </v-card-item>
  </v-card>
</template>
