<script setup lang="ts">
let tracks = ref([
  {
    title: "прикол1",
    year: "2021",
    artist_id: "Название исполнителя",
    album_id: "Название альбома"
  },
  {
    title: "прикол2",
    year: "2021",
    artist_id: "Название исполнителя",
    album_id: "Название альбома"
  },
])
</script>
<template>
  <v-card v-for="track of tracks">
    {{ track.title }}
    {{ track.year }}
    {{ track.artist_id }}
    {{ track.album_id }}
  </v-card>
</template>