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
  <v-card :elevation="0">
    <v-card-item>
      <v-card-title
        ><NuxtLink to="/album">{{ album.album_name }}</NuxtLink></v-card-title
      >
      <v-card-subtitle>
        <NuxtLink to="/artist">{{ album.artist_name }}</NuxtLink>
      </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      {{ album.year }}
      {{ album.genre_name }}
    </v-card-text>

    <!-- <template class="d-flex justify-center" v-slot:actions>
      <v-btn prepend-icon="mdi-pencil">редактировать</v-btn>
      <v-btn prepend-icon="mdi-delete">удалить</v-btn>
    </template> -->
  </v-card>
</template>
