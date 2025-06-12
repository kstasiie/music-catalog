<script setup>
let album = ref({
  title: "Альбом1",
  year: "2020",
  artist_id: "artist_1",
  genre_id: "rock",
  tracks: [
    {
      title: "Song One",
      year: "2020",
      artist_id: "artist_1",
      genre_id: "rock",
    },
    {
      title: "Song Two",
      year: "2020",
      artist_id: "artist_1",
      genre_id: "rock",
    },
  ],
});
</script>
<template>
  <v-container class="text-center">
    <v-row justify="center" class="controls">
      <v-col cols="12" sm="9" class="">
        <v-card :elevation="0" class="mb-4">
          <v-card-item>
            <v-card-title>{{ album.title }}</v-card-title>
            <v-card-subtitle> </v-card-subtitle>
          </v-card-item>
          <v-card-text>
            <div>Released: {{ album.year }}</div>
            <div>Genre: {{ album.genre_id }}</div>
            <v-row>
              <v-col cols="6" v-for="track in album.tracks" :key="track.title">
                <TrackCard :track="track"></TrackCard>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
