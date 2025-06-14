<script setup lang="ts">
defineProps<{
  track: {
    name: string;
    year: string;
    artist_name: string;
    album_name: string;
    genre_name: string;
  };
}>();
</script>
<template>
  <v-card :elevation="4">
    <v-card-item>
      <v-card-title class="text-h5 font-weight-black">
        {{ track.name }}
      </v-card-title>

      <v-card-subtitle class="text-h6 font-weight-black">
        <NuxtLink to="/artist">{{ track.artist_name }}</NuxtLink>
      </v-card-subtitle>
      <NuxtLink
        class="font-weight-medium"
        :to="`/album/${encodeURIComponent(track.album_name)}`"
        >{{ track.album_name }}</NuxtLink
      >
    </v-card-item>
    <template v-slot:actions>
      <v-row class="d-flex justify-center">
        <v-col>
          <NuxtLink to="/add-track"
            ><v-btn class="font-weight-medium" prepend-icon="mdi-pencil"
              >редактировать</v-btn
            ></NuxtLink
          >
          <v-btn class="font-weight-medium" prepend-icon="mdi-delete"
            >удалить</v-btn
          >
        </v-col>
      </v-row>
    </template>
  </v-card>
</template>
