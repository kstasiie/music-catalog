<script setup lang="ts">
let artists = ref([
  {
    name: "Имя исполнителя1",
    description: "Описание",
    artist_id: "1",
  },
  {
    name: "Имя исполнителя2",
    description: "Описание",
    artist_id: "2",
  },
]);
</script>
<template>
  <v-row>
    <v-col cols="12" v-for="(item, index) of artists" :key="index">
      <ArtistCard :artist="item" />
    </v-col>
  </v-row>
</template>
