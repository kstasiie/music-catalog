<script setup lang="ts">
import { ref } from "vue";

const form = ref({
  artist: "",
  album: "",
  title: "",
  genre: "",
  year: "",
});

async function submit() {
  try {
    const params = new URLSearchParams();
    params.append("artist", form.value.artist);
    params.append("title", form.value.title);
    if (form.value.genre) params.append("genre", form.value.genre);
    if (form.value.album) params.append("album", form.value.album);
    if (form.value.year) params.append("year", form.value.year);

    const response = await fetch(
      `http://127.0.0.1:8000/songs?${params.toString()}`,
      {
        method: "POST",
        mode: "cors",
      }
    );

    if (!response.ok) {
      const errorData = await response.json();
      console.error(
        "Ошибка добавления трека:",
        errorData.detail || response.statusText
      );
      return;
    }

    alert("Трек успешно добавлен!");
    // Очистить форму
    form.value = {
      artist: "",
      album: "",
      title: "",
      genre: "",
      year: "",
    };
  } catch (error) {
    console.error("Ошибка при запросе:", error);
  }
}
</script>
<template>
  <v-sheet
    rounded
    class="mx-auto"
    width="500"
    elevation="4"
    variant="elevated"
    color="blue-lighten-5"
  >
    <v-form @submit.prevent="submit">
      <v-text-field
        rounded
        variant="solo-filled"
        label="Имя исполнителя"
        v-model="form.artist"
        required
      ></v-text-field>

      <v-text-field
        rounded
        variant="solo-filled"
        label="Название альбома"
        v-model="form.album"
      ></v-text-field>

      <v-text-field
        rounded
        variant="solo-filled"
        label="Название песни"
        v-model="form.title"
        required
      ></v-text-field>

      <v-text-field
        rounded
        variant="solo-filled"
        label="Жанр"
        v-model="form.genre"
      ></v-text-field>

      <v-text-field
        rounded
        variant="solo-filled"
        label="Год выпуска"
        v-model="form.year"
      ></v-text-field>

      <v-btn rounded class="mt-2" type="submit" block>Добавить</v-btn>
    </v-form>
  </v-sheet>
</template>
