<script setup lang="ts">
import { ref, onMounted, watch } from "vue";
import { useRoute } from "vue-router";
// Убедитесь, что путь к компоненту TrackCard правильный для вашей структуры проекта
import TrackCard from "~/components/TrackCard.vue";

const route = useRoute();

// Получаем параметр по имени 'album_name', как его ожидает бэкенд и новый маршрут Nuxt.js
const albumName = ref((route.params.album_name as string) || "");

const album = ref({
  title: "",
  year: "", // В бэкенде это description, но используем 'year' для согласованности с фронтендом
  artist_id: "",
  artist_name: "", // Добавляем artist_name для отображения и передачи в TrackCard
  tracks: [] as Array<{
    title: string;
    year: number;
    artist_id: string; // ID исполнителя, который может быть взят из альбома
    genre_id: string; // Возможно, будет пустой строкой, если API не возвращает
  }>,
});

async function fetchAlbumDetails() {
  // Используем albumName для запроса к API
  if (!albumName.value) {
    console.error("Название альбома не задано в маршруте.");
    return;
  }

  try {
    const response = await fetch(
      // Запрос к бэкенду использует название альбома
      `http://127.0.0.1:8000/albums/${encodeURIComponent(albumName.value)}`,
      {
        method: "GET",
        headers: { "Content-Type": "application/json" },
      }
    );

    if (!response.ok) {
      console.error(
        `Ошибка загрузки альбома: ${response.status} - ${response.statusText}`
      );
      // Если альбом не найден, можно показать сообщение или перенаправить
      if (response.status === 404) {
        console.warn(`Альбом "${albumName.value}" не найден.`);
      }
      return;
    }

    const data = await response.json();

    // Проверяем, что полученные данные соответствуют ожидаемой структуре
    if (data && data.album && data.songs) {
      album.value = {
        title: data.album.name,
        year: data.album.description || "", // Используем description как year
        artist_id: data.album.artist_id,
        artist_name: data.album.artist_name, // Получаем имя исполнителя
        tracks: data.songs.map((song: any) => ({
          title: song.title,
          year: song.year,
          artist_id: data.album.artist_id, // Берем artist_id из альбома, т.к. бэкенд не возвращает его для каждой песни
          genre_id: "", // Бэкенд не возвращает genre_id для песен в этом запросе
        })),
      };
    } else {
      console.error("Неверная структура данных от API:", data);
    }
  } catch (error) {
    console.error("Ошибка запроса:", error);
  }
}

// Запускаем загрузку данных при монтировании компонента
onMounted(fetchAlbumDetails);

// Добавляем вотчер для отслеживания изменений в параметрах маршрута.
// Это важно, если пользователь переходит между страницами альбомов без полной перезагрузки.
watch(
  // Отслеживаем изменение route.params.album_name
  () => route.params.album_name,
  (newAlbumName) => {
    if (newAlbumName) {
      albumName.value = newAlbumName as string;
      fetchAlbumDetails();
    }
  },
  { immediate: true } // immediate: true гарантирует, что fetchAlbumDetails будет вызван сразу после инициализации
);
</script>

<template>
  <v-container class="text-center">
    <h1 class="text-h4 font-weight-black">{{ album.title }}</h1>

    <v-row justify="center" class="controls">
      <v-col cols="12" sm="9">
        <br />
        <v-card :elevation="4" variant="elevated" color="blue-lighten-5">
          <v-card-item>
            <v-card-subtitle class="text-h5 font-weight-black"
              >Исполнитель: {{ album.artist_name }}</v-card-subtitle
            >
          </v-card-item>
          <v-card-text>
            <v-row class="d-flex justify-center">
              <v-col
                cols="12"
                sm="11"
                lg="11"
                xl="11"
                v-for="track in album.tracks"
                :key="track.title"
              >
                <TrackCard
                  :track="{
                    name: track.title,
                    year: track.year.toString(),
                    artist_name: album.artist_name, // или правильное имя исполнителя
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
