<script setup>
import { ref } from "vue";

let selectedTab = ref("Track");

function setTab(tab) {
  selectedTab.value = tab;
}
</script>

<template>
  <v-app>
    <v-main class="music-catalog">
      <v-container class="text-center">
        <img src="~/assets/images/main-logo.svg" alt="">
        <v-row justify="center" class="controls">
          <v-col cols="12" sm="9" lg="6" xl="4" class="d-flex">
            <v-btn :color="selectedTab === 'Track' ? 'blue lighten-2' : 'white'" width="30%"
              :class="{ 'white--text': selectedTab === 'Track' }" class="mx-2" @click="setTab('Track')">
              Трек
            </v-btn>
            <v-btn :color="selectedTab === 'Album' ? 'blue lighten-2' : 'white'" width="30%"
              :class="{ 'white--text': selectedTab === 'Album' }" class="mx-2" @click="setTab('Album')">
              Альбом
            </v-btn>
            <v-btn :color="selectedTab === 'Artist' ? 'blue lighten-2' : 'white'" width="30%"
              :class="{ 'white--text': selectedTab === 'Artist' }" class="mx-2" @click="setTab('Artist')">
              Исполнитель
            </v-btn>
          </v-col>
        </v-row>

        <v-row class="d-flex justify-center">
          <v-col cols="12" sm="9" lg="6" xl="4">
            <v-text-field v-model="searchQuery" placeholder="Введите" rounded variant="solo" class="search-input"
              append-inner-icon="mdi-magnify"></v-text-field>
          </v-col>
        </v-row>

        <v-row class="d-flex justify-center">
          <v-col cols="7">
            <Tracks v-if="selectedTab == 'Track'" />
            <Albums v-else-if="selectedTab == 'Album'" />
            <Artists v-else-if="selectedTab == 'Artist'" />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<style lang="scss" scoped>
.music-catalog {
  background: linear-gradient(to bottom, #87ceeb, #1e90ff);
  height: 100vh;
  display: flex;
  // align-items: center;
  justify-content: center;
}

.title {
  color: white;
  font-size: 4em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  font-family: Arial, sans-serif;
}

.controls {
  margin: 20px 0;
}
</style>
