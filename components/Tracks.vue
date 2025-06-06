<script setup lang="ts">
let tracks = ref([
  {
    title: "прикол1",
    year: "2021",
    artist_id: "Название исполнителя",
    album_id: "Название альбома",
  },
  {
    title: "прикол2",
    year: "2021",
    artist_id: "Название исполнителя",
    album_id: "Название альбома",
  },
]);
</script>
<template>
  <v-card
    v-for="track of tracks"
    class="mx-auto my-8"
    elevation="16"
    max-width="300"
    ><!--{{ track.title }}
    {{ track.year }}
    {{ track.artist_id }}
    {{ track.album_id }}-->
    <v-card-item>
      <v-card-title> {{ track.title }} </v-card-title>

      <v-card-subtitle> {{ track.artist_id }} </v-card-subtitle>
    </v-card-item>

    <v-card-text> {{ track.year }} {{ track.album_id }} </v-card-text>
  </v-card>
</template>
