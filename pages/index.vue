<script setup>
// Add functionality if needed, e.g., reactive state for the input
import { ref } from "vue";

const searchQuery = ref("");
const selectedTab = ref("Track");

function setTab(tab) {
  selectedTab.value = tab;
}
</script>

<template>
  <div class="music-catalog">
    <h1>MUSIC catalog</h1>
    <div class="controls">
      <button
        :class="{ active: selectedTab === 'Track' }"
        @click="setTab('Track')"
      >
        Track
      </button>
      <button
        :class="{ active: selectedTab === 'Album' }"
        @click="setTab('Album')"
      >
        Album
      </button>
      <button
        :class="{ active: selectedTab === 'Playlist' }"
        @click="setTab('Playlist')"
      >
        Playlist
      </button>
    </div>
    <input v-model="searchQuery" type="text" placeholder="Song 1" />
  </div>
</template>

<style lang="scss" scoped>
.music-catalog {
  background: linear-gradient(to bottom, #87ceeb, #1e90ff);
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  font-family: Arial, sans-serif;
}

h1 {
  font-size: 4em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.controls {
  margin: 20px 0;
}

button {
  padding: 10px 20px;
  margin: 0 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background-color: white;
  color: black;
  transition: background-color 0.3s;

  &.active {
    background-color: #6495ed;
    color: white;
  }
}

input {
  padding: 10px;
  width: 300px;
  border: none;
  border-radius: 10px;
  font-size: 1em;
  background-color: rgba(255, 255, 255, 0.9);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);

  &::placeholder {
    color: #888;
  }
}
</style>
