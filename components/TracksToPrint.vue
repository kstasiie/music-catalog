<script setup lang="ts">
let tracks = ref([]);

let query = ref("");
async function fetchTracks() {
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
      // the response isn't okay
      console.log("response status isn't 200");
      return;
    }
    const data = await response.json();
    // result.alternatives[0].message.text -- text of model response
    console.log(data);
    if (data.results != null) {
      tracks.value = data.results;
    }
  } catch (error) {
    console.log(error);
  }
}

fetchTracks();
async function exportSongs() {
  try {
    // Просто перейдите по URL, чтобы инициировать скачивание.
    // Браузер сам обработает скачивание файла.
    window.open("http://127.0.0.1:8000/export/songs/docx", "_blank");
    // "_blank" откроет новую вкладку, чтобы не прерывать текущую страницу
  } catch (error) {
    console.error("Ошибка при экспорте DOCX:", error);
    alert("Произошла ошибка при экспорте песен.");
  }
}
</script>
<template>
  <v-btn
    rounded
    class="mt-4"
    type="submit"
    block
    color="primary"
    @click="exportSongs"
  >
    Экспортировать в DOCX
    <v-icon right>mdi-file-word</v-icon>
  </v-btn>
  <br />
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
