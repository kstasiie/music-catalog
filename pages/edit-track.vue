<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

// Инициализируем useRoute для доступа к параметрам маршрута
const route = useRoute();

const form = ref({
  artist: "",
  album: "",
  title: "",
  genre: "",
  year: "",
});

const originalTrackTitle = ref("");

// На этой странице мы всегда в режиме редактирования
const isEditMode = ref(true);

onMounted(() => {
  if (route.query.edit === "true" && route.query.originalTitle) {
    // Сохраняем оригинальное название трека, которое будет использоваться в URL для PUT-запроса
    originalTrackTitle.value = route.query.originalTitle as string;

    // Предзаполняем форму данными из параметров запроса
    form.value.artist = (route.query.artist as string) || "";
    form.value.album = (route.query.album as string) || "";
    form.value.title = (route.query.name as string) || "";
    form.value.genre = (route.query.genre as string) || "";
    form.value.year = (route.query.year as string) || "";
  } else {
    console.error(
      "Ошибка: Страница редактирования открыта без необходимых параметров трека."
    );
    alert(
      "Ошибка: Не удалось загрузить данные для редактирования. Убедитесь, что вы перешли по ссылке редактирования трека."
    );
  }
});

async function submit() {
  try {
    let apiUrl: string;
    let successMessage: string;
    let errorMessagePrefix: string;
    const requestParams = new URLSearchParams();

    const fetchOptions: RequestInit = {
      method: "PUT",
      mode: "cors",
      headers: {},
    };

    // Логика для режима редактирования (PUT-запрос)
    if (!originalTrackTitle.value) {
      alert(
        "Ошибка: Оригинальное название трека отсутствует для редактирования."
      );
      return;
    }
    apiUrl = `http://127.0.0.1:8000/songs/${encodeURIComponent(
      originalTrackTitle.value
    )}`;
    successMessage = "Трек успешно обновлен!";
    errorMessagePrefix = "Ошибка обновления трека:";

    if (form.value.title) requestParams.append("new_title", form.value.title);
    if (form.value.artist)
      requestParams.append("new_artist", form.value.artist);

    requestParams.append("new_genre", form.value.genre);
    requestParams.append("new_album", form.value.album);

    if (form.value.year) requestParams.append("new_year", form.value.year);

    // Параметры для PUT-запроса добавляются к URL
    apiUrl += `?${requestParams.toString()}`;

    const response = await fetch(apiUrl, fetchOptions);

    if (!response.ok) {
      const errorData = await response.json();
      console.error(
        errorMessagePrefix,
        errorData.detail || response.statusText
      );
      alert(
        `${errorMessagePrefix} ${errorData.detail || "Неизвестная ошибка"}`
      );
      return;
    }

    alert(successMessage);
    const router = useRouter();
    if (form.value.album) {
      router.push({
        name: "album-album_name",
        params: { album_name: form.value.album },
      });
    } else {
      router.push("/");
    }
  } catch (error) {
    console.error("Ошибка при запросе:", error);
    alert("Ошибка при запросе. Попробуйте еще раз.");
  }
}
</script>

<template>
  <v-container class="text-center">
    <v-row justify="center" class="controls">
      <v-col cols="12" sm="9" lg="6" xl="4" class="d-flex">
        <v-sheet
          rounded
          class="mx-auto pa-4"
          width="500"
          elevation="4"
          variant="elevated"
          color="blue-lighten-5"
        >
          <v-form @submit.prevent="submit">
            <h2 class="text-h4 font-weight-black">Редактировать трек</h2>
            <br />
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

            <v-btn rounded class="mt-4" type="submit" block color="primary">
              Обновить
            </v-btn>
          </v-form>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
