<script setup lang="ts">
import { ref, onMounted, watch } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();

// Получаем имя артиста из параметров маршрута
const artistName = ref((route.params.artist_name as string) || "");

// Хранилище для информации об артисте и его альбомах
const artistDetails = ref({
  name: "",
  albums: [] as Array<{
    id: string;
    name: string;
    description: string;
  }>,
});

async function fetchArtistDetails() {
  console.log("Попытка загрузить детали для артиста:", artistName.value);

  if (!artistName.value) {
    console.error("Имя артиста не задано в маршруте, запрос к API отменен.");
    return;
  }

  try {
    const response = await fetch(
      // Запрос к бэкенду для получения альбомов артиста
      `http://127.0.0.1:8000/artists/${encodeURIComponent(
        artistName.value
      )}/albums`,
      {
        method: "GET",
        headers: { "Content-Type": "application/json" },
      }
    );

    if (!response.ok) {
      console.error(
        `Ошибка загрузки альбомов артиста: ${response.status} - ${response.statusText}`
      );
      if (response.status === 404) {
        console.warn(
          `Артист "${artistName.value}" не найден или у него нет альбомов.`
        );
      }
      return;
    }

    const data = await response.json();

    if (data && Array.isArray(data.albums)) {
      artistDetails.value.name = artistName.value; // Имя артиста получаем из маршрута
      artistDetails.value.albums = data.albums.map((album: any) => ({
        id: album.id,
        name: album.name,
        description: album.description || "", // Используем описание альбома
      }));
    } else {
      console.error(
        "Неверная структура данных от API для альбомов артиста:",
        data
      );
    }
  } catch (error) {
    console.error("Ошибка запроса:", error);
  }
}

onMounted(fetchArtistDetails);

// Добавляем вотчер для отслеживания изменений в параметрах маршрута.
watch(
  () => route.params.artist_name,
  (newArtistName) => {
    if (newArtistName) {
      artistName.value = newArtistName as string;
      fetchArtistDetails();
    } else {
      console.warn("Параметр artist_name в маршруте не найден или undefined.");
      artistDetails.value = {
        // Очищаем данные, если параметр недействителен
        name: "",
        albums: [],
      };
    }
  },
  { immediate: true }
);
</script>

<template>
  <v-container class="text-center">
    <h1 class="text-h4 font-weight-black">{{ artistDetails.name }}</h1>
    <v-row justify="center" class="controls">
      <v-col cols="12" sm="9">
        <br />
        <v-card :elevation="4" variant="elevated" color="blue-lighten-5">
          <v-card-item>
            <v-card-subtitle class="text-h5 font-weight-black"
              >Альбомы</v-card-subtitle
            >
          </v-card-item>
          <v-card-text>
            <v-row
              class="d-flex justify-center"
              v-if="artistDetails.albums.length > 0"
            >
              <!-- Проходим по альбомам артиста -->
              <v-col v-for="album in artistDetails.albums" :key="album.name">
                <v-card :elevation="4">
                  <v-card-title class="text-h5 font-weight-black"
                    ><NuxtLink
                      :to="{
                        name: 'album-album_name',
                        params: { album_name: album.name },
                      }"
                      class="text-h5 font-weight-black"
                      >{{ album.name }}</NuxtLink
                    ></v-card-title
                  >
                </v-card>
              </v-col>
            </v-row>
            <div v-else class="text-body-1 text-grey-darken-1">
              Альбомы этого артиста не найдены.
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
