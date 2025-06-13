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


let query = ref('')
async function fetchTracks() {

  try {
    let response = await fetch(`http://127.0.0.1:8000/search?query=${query.value}`, {
      method: "GET",
      mode: "cors",
      headers: {
        "Content-Type": "application/json"
      },
    });

    if (!response.ok) {
      // the response isn't okay
      console.log("response status isn't 200");
      return;
    }
    const data = await response.json();
    // result.alternatives[0].message.text -- text of model response
    console.log(data);

  } catch (error) {
    console.log(error);
  }
}
fetchTracks();
</script>
<template>
  <v-card elevation="4" variant="elevated" color="blue-lighten-5">
    <v-row>
      <v-col class="d-flex justify-center" cols="12" v-for="(item, index) of tracks" :key="index">
        <TrackCard :track="item" />
      </v-col>
    </v-row>
  </v-card>
</template>
