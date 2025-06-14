<script setup lang="ts">
let albums = ref([]);

let query = ref("");
async function fetchAlbums() {
  try {
    let response = await fetch(
      `http://127.0.0.1:8000/search?query=${query.value}`,
      {
        method: "GET",
        mode: "cors",
        headers: {
          "Content-Type": "application/json",
        },
      }
    );

    if (!response.ok) {
      console.log("response status isn't 200");
      return;
    }
    const data = await response.json();

    console.log(data);
    if (data.results != null) {
      albums.value = data.results;
    }
  } catch (error) {
    console.log(error);
  }
}
function submit() {
  fetchAlbums();
}
fetchAlbums();
</script>
<template>
  <v-row class="d-flex justify-center">
    <v-col>
      <v-text-field
        placeholder="Введите название песни"
        rounded
        variant="solo"
        class="search-input"
        append-inner-icon="mdi-magnify"
        v-model="query"
        @click:append-inner="submit"
      ></v-text-field>
    </v-col>
  </v-row>
  <v-card elevation="4" variant="elevated" color="blue-lighten-5">
    <v-row class="d-flex justify-center">
      <v-col
        cols="12"
        sm="11"
        lg="11"
        xl="11"
        v-for="(item, index) of albums"
        :key="index"
      >
        <AlbumCard :album="item" />
      </v-col>
    </v-row>
  </v-card>
</template>
