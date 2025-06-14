<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router"; // Импортируем useRoute

// Инициализируем useRoute для доступа к параметрам маршрута
const route = useRoute();

const form = ref({
  artist: "",
  album: "",
  title: "",
  genre: "",
  year: "",
});

// Храним оригинальное название трека для запросов PUT (редактирование)
const originalTrackTitle = ref("");

// На этой странице мы всегда в режиме редактирования
const isEditMode = ref(true);

onMounted(() => {
  // Поскольку эта страница предназначена только для редактирования, мы всегда ожидаем параметры
  // Проверяем наличие 'edit=true' и 'originalTitle' для надежности, хотя 'edit' можно было бы и не проверять,
  // если маршрутизация гарантирует это.
  if (route.query.edit === "true" && route.query.originalTitle) {
    // Сохраняем оригинальное название трека, которое будет использоваться в URL для PUT-запроса
    originalTrackTitle.value = route.query.originalTitle as string;

    // Предзаполняем форму данными из параметров запроса
    form.value.artist = (route.query.artist as string) || "";
    form.value.album = (route.query.album as string) || "";
    form.value.title = (route.query.name as string) || ""; // 'name' из TrackCard — это 'title' здесь
    form.value.genre = (route.query.genre as string) || "";
    form.value.year = (route.query.year as string) || "";
  } else {
    // Если параметры для редактирования отсутствуют, выводим ошибку или перенаправляем
    console.error(
      "Ошибка: Страница редактирования открыта без необходимых параметров трека."
    );
    alert(
      "Ошибка: Не удалось загрузить данные для редактирования. Убедитесь, что вы перешли по ссылке редактирования трека."
    );
    // В реальном приложении здесь можно было бы перенаправить на главную страницу или страницу со списком треков
    // const router = useRouter();
    // router.push('/');
  }
});

async function submit() {
  try {
    let apiUrl: string;
    let successMessage: string;
    let errorMessagePrefix: string;
    const requestParams = new URLSearchParams();

    const fetchOptions: RequestInit = {
      method: "PUT", // Метод всегда PUT для редактирования
      mode: "cors", // Убедитесь, что CORS правильно настроен на вашем бэкенде
      headers: {}, // Заголовки будут пустыми, так как параметры идут в URL
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

    // Добавляем параметры для PUT-запроса, используя префикс 'new_' как ожидается бэкендом
    // Добавляем только те поля, которые имеют значение (или если они явно очищены)
    if (form.value.title) requestParams.append("new_title", form.value.title);
    if (form.value.artist)
      requestParams.append("new_artist", form.value.artist);

    // Для жанра и альбома, если поле очищено (пустая строка), отправляем это явно
    requestParams.append("new_genre", form.value.genre);
    requestParams.append("new_album", form.value.album);

    // Год для PUT запроса также передается как строка в параметрах
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
              <!-- Текст кнопки всегда "Обновить" -->
              Обновить
            </v-btn>
          </v-form>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
