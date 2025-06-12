<script setup lang="ts">
let albums = ref([
  {
    title: "альбом1",
    year: "2021",
    artist_id: "Имя исполнителя",
    genre_id: "Жанр",
  },
  {
    title: "альбом1",
    year: "2021",
    artist_id: "Имя исполнителя",
    genre_id: "Жанр",
  },
]);
</script>
<template>
  <v-row>
    <v-col cols="12" v-for="(item, index) of albums" :key="index">
      <AlbumCard :album="item" />
    </v-col>
  </v-row>
</template>
