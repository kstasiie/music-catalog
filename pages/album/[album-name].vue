<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();

const albumName = ref((route.params.album_name as string) || "");

const album = ref({
  title: "",
  year: "",
  artist_id: "",
  tracks: [] as Array<{
    title: string;
    year: number;
    artist_id: string;
    genre_id: string;
  }>,
});

async function fetchAlbumDetails() {
  if (!albumName.value) {
    console.error("Название альбома не задано в маршруте");
    return;
  }

  try {
    const response = await fetch(
      `http://127.0.0.1:8000/albums/${encodeURIComponent(albumName.value)}`,
      {
        method: "GET",
        headers: { "Content-Type": "application/json" },
      }
    );

    if (!response.ok) {
      console.error("Ошибка загрузки альбома:", response.statusText);
      return;
    }

    const data = await response.json();

    album.value = {
      title: data.album.name,
      year: data.album.description || "",
      artist_id: data.album.artist_id,
      tracks: data.songs.map((song: any) => ({
        title: song.title,
        year: song.year,
        artist_id: data.album.artist_id,
        genre_id: "",
      })),
    };
  } catch (error) {
    console.error("Ошибка запроса:", error);
  }
}

onMounted(fetchAlbumDetails);
</script>

<template>
  <v-container class="text-center">
    <v-row justify="center" class="controls">
      <v-col cols="12" sm="9">
        <v-card :elevation="0" class="mb-4">
          <v-card-item>
            <v-card-title>{{ album.title }}</v-card-title>
            <v-card-subtitle>Artist ID: {{ album.artist_id }}</v-card-subtitle>
          </v-card-item>
          <v-card-text>
            <div>Released: {{ album.year }}</div>
            <v-row>
              <v-col cols="6" v-for="track in album.tracks" :key="track.title">
                <TrackCard
                  :track="{
                    name: track.title,
                    year: track.year.toString(),
                    artist_name: album.artist_id, // или правильное имя исполнителя
                    album_name: album.title, // или album.name, если нужно
                    genre_name: track.genre_id, // или album.genre_name, если есть
                  }"
                />
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
