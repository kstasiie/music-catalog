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
  <v-card :elevation="4">
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
    <template class="d-flex justify-center" v-slot:actions>
      <NuxtLink to="/add-track"
        ><v-btn prepend-icon="mdi-pencil">редактировать</v-btn></NuxtLink
      >
      <v-btn prepend-icon="mdi-delete">удалить</v-btn>
    </template>
  </v-card>
</template>
