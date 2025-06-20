<script setup lang="ts">
import { ref } from "vue";

// Пропсы ожидаемых значений
const props = defineProps<{
  track: {
    name: string;
    year: string;
    artist_name: string;
    album_name: string;
    genre_name: string;
  };
}>();

const emit = defineEmits(["track-deleted", "delete-error"]);

const isDeleting = ref(false);

// удаление трека
async function handleDeleteTrack() {
  if (!props.track.name) {
    console.error("Название трека отсутствует. Невозможно удалить.");
    alert("Ошибка: Невозможно удалить трек без названия.");
    return;
  }

  // подтверждение от пользователя
  if (!confirm(`Вы уверены, что хотите удалить трек "${props.track.name}"?`)) {
    return;
  }

  isDeleting.value = true;

  try {
    const response = await fetch(
      `http://127.0.0.1:8000/songs/${encodeURIComponent(props.track.name)}`, // запрос на веб-сервер
      {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
        },
      }
    );

    if (response.ok) {
      const data = await response.json();
      alert(`Трек "${props.track.name}" успешно удален: ${data.message}`);
      emit("track-deleted", props.track.name);
    } else {
      const errorData = await response.json();
      console.error(
        `Ошибка при удалении трека "${props.track.name}":`,
        errorData
      );
      alert(
        `Ошибка при удалении трека "${props.track.name}": ${
          errorData.detail || "Неизвестная ошибка"
        }`
      );
      emit("delete-error", {
        trackName: props.track.name,
        error: errorData.detail || "Неизвестная ошибка",
      });
    }
  } catch (error) {
    console.error(
      `Ошибка сети или сервера при удалении трека "${props.track.name}":`,
      error
    );
    alert(
      `Ошибка сети/сервера при удалении трека "${props.track.name}". Попробуйте еще раз.`
    );
    emit("delete-error", { trackName: props.track.name, error: error });
  } finally {
    isDeleting.value = false;
  }
}
</script>

<template>
  <v-card :elevation="4">
    <v-card-item>
      <v-card-title class="text-h5 font-weight-black">
        {{ track.name }}
      </v-card-title>

      <v-card-subtitle class="text-h6 font-weight-black">
        <NuxtLink
          class="font-weight-black"
          :to="{
            name: 'artist-artist_name',
            params: { artist_name: track.artist_name },
          }"
        >
          {{ track.artist_name }}
        </NuxtLink>
      </v-card-subtitle>
      <NuxtLink
        class="text-h7 font-weight-black"
        v-if="track.album_name != null"
        :to="{
          name: 'album-album_name',
          params: { album_name: track.album_name },
        }"
        >{{ track.album_name }}</NuxtLink
      >
    </v-card-item>
    <template v-slot:actions>
      <v-row class="d-flex justify-center">
        <v-col>
          <!-- Кнопка "редактировать" использует проп 'to' напрямую -->
          <v-btn
            class="font-weight-medium"
            prepend-icon="mdi-pencil"
            :to="{
              path: '/edit-track',
              query: {
                edit: 'true',
                originalTitle: track.name,
                name: track.name,
                artist: track.artist_name,
                album: track.album_name,
                genre: track.genre_name,
                year: track.year,
              },
            }"
          >
            редактировать
          </v-btn>
          <v-btn
            class="font-weight-medium"
            prepend-icon="mdi-delete"
            color="error"
            @click="handleDeleteTrack"
            :loading="isDeleting"
            :disabled="isDeleting"
          >
            удалить
          </v-btn>
        </v-col>
      </v-row>
    </template>
  </v-card>
</template>
