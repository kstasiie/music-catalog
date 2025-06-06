<script setup lang="ts">
let tracks = ref([
  {
    title: "прикол1",
    year: "2021",
    artist_id: "Имя исполнителя",
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
  <v-row>
    <v-col cols="12" v-for="(item, index) of tracks" :key="index">
      <TrackCard :track="item" />
    </v-col>
  </v-row>
</template>
