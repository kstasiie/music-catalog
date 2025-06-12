<script setup lang="ts">
defineProps<{
  album: {
    title: string;
    year: string;
    artist_id: string;
    genre_id: string;
  };
}>();
</script>

<template>
  <v-card :elevation="0">
    <v-card-item>
      <v-card-title
        >{{ album.title
        }}<v-btn icon="mdi-pencil-outline" size="x-small"></v-btn
      ></v-card-title>
      <!-- <v-btn icon="mdi-pencil-outline" size="x-small"></v-btn> -->

      <v-card-subtitle>
        <NuxtLink to="/artist">{{ album.artist_id }}</NuxtLink>
      </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      {{ album.year }}
      {{ album.genre_id }}
    </v-card-text>

    <template v-slot:actions>
      <v-btn prepend-icon="mdi-pencil">редактировать</v-btn>
    </template>
  </v-card>
</template>
