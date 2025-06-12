<script setup lang="ts">
const artist = ref<any>({
  name: "Имя исполнителя",
  id: "artist_1",
  albums: [
    {
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
    },
    {
      title: "Альбом2",
      year: "2022",
      artist_id: "artist_1",
      genre_id: "pop",
      tracks: [
        {
          title: "Song Three",
          year: "2022",
          artist_id: "artist_1",
          genre_id: "pop",
        },
        {
          title: "Song Four",
          year: "2022",
          artist_id: "artist_1",
          genre_id: "pop",
        },
      ],
    },
  ],
});
</script>

<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>{{ artist.name }}</h1>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <h2>Альбомы</h2>
        <v-list>
          <v-list-item
            v-for="album in artist.albums"
            :key="album.title + album.year"
          >
            <v-card :elevation="0" class="mb-4">
              <v-card-item>
                <v-card-title>{{ album.title }}</v-card-title>
                <v-card-subtitle> </v-card-subtitle>
              </v-card-item>
              <v-card-text>
                <div>Released: {{ album.year }}</div>
                <div>Genre: {{ album.genre_id }}</div>
                <v-list dense>
                  <v-subheader>Tracks</v-subheader>
                  <v-list-item v-for="track in album.tracks" :key="track.title">
                    <v-list-item-title>{{ track.title }}</v-list-item-title>
                    <v-list-item-subtitle
                      >{{ track.year }} |
                      {{ track.genre_id }}</v-list-item-subtitle
                    >
                  </v-list-item>
                </v-list>
              </v-card-text>
            </v-card>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped>
h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}
h2 {
  font-size: 1.8rem;
  margin-top: 2rem;
  margin-bottom: 1rem;
}
</style>
