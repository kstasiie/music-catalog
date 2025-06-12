<script setup>
let form = ref({
  title: "",
  artist: "",
  genre: "",
  album: "",
  year: "",
});

function submit() {}
</script>
<template>
  <v-sheet class="mx-auto" width="500"
    >{{ form }}
    <v-form fast-fail @submit.prevent="submit">
      <v-text-field
        variant="solo-filled"
        label="Имя исполнителя"
        v-model="form.artist"
      ></v-text-field>

      <v-text-field
        variant="solo-filled"
        label="Название альбома"
        v-model="form.album"
      ></v-text-field>
      <v-text-field
        variant="solo-filled"
        label="Название песни"
        v-model="form.title"
      ></v-text-field>
      <v-text-field
        variant="solo-filled"
        label="Жанр"
        v-model="form.genre"
      ></v-text-field>
      <v-text-field
        variant="solo-filled"
        label="Год выпуска"
        v-model="form.year"
      ></v-text-field>
      <v-btn class="mt-2" type="submit" block>Добавить</v-btn>
    </v-form>
  </v-sheet>
</template>
