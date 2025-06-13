<script setup lang="ts">
defineProps<{
  track: {
    title: string;
    year: string;
    artist_name: string;
    album_name: string;
  };
}>();
</script>
<template>
  <v-card :elevation="4">
    <v-card-item>
      <v-card-title> {{ track.title }} </v-card-title>

      <v-card-subtitle>
        <NuxtLink to="/artist">{{ track.artist_name }}</NuxtLink>
      </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      {{ track.year }}
      <NuxtLink to="/album">{{ track.album_name }}</NuxtLink>
    </v-card-text>
    <template v-slot:actions>
      <v-row class="d-flex justify-center">
        <v-col>
          <NuxtLink to="/add-track"
            ><v-btn prepend-icon="mdi-pencil">редактировать</v-btn></NuxtLink
          >
          <v-btn prepend-icon="mdi-delete">удалить</v-btn>
        </v-col>
      </v-row>
    </template>
  </v-card>
</template>
