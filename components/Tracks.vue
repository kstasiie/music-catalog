<script setup lang="ts">
let tracks = ref([]);

let query = ref("");
async function fetchTracks() {
  try {
    // запрос к серверу
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
      tracks.value = data.results;
    }
  } catch (error) {
    console.log(error);
  }
}

function submit() {
  fetchTracks();
}
fetchTracks();
</script>
<template>
  <v-row class="d-flex justify-center">
    <v-col>
      <v-text-field
        placeholder="Введите трек, альбом или исполнителя"
        rounded
        variant="solo"
        class="search-input"
        append-inner-icon="mdi-magnify"
        v-model="query"
        @click:append-inner="submit"
      ></v-text-field>
    </v-col>
  </v-row>

  <v-sheet
    rounded
    class="mx-auto pa-4"
    elevation="4"
    variant="elevated"
    color="blue-lighten-5"
  >
    <v-row class="d-flex justify-center">
      <v-col
        cols="12"
        sm="11"
        lg="11"
        xl="11"
        v-for="(item, index) of tracks"
        :key="index"
      >
        <TrackCard :track="item" />
      </v-col>
    </v-row>
  </v-sheet>
</template>
