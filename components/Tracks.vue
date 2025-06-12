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
  <v-card elevation="4" variant="elevated" color="blue-lighten-5">
    <v-row>
      <v-col
        class="d-flex justify-center"
        cols="12"
        v-for="(item, index) of tracks"
        :key="index"
      >
        <TrackCard :track="item" />
      </v-col>
    </v-row>
  </v-card>
</template>
