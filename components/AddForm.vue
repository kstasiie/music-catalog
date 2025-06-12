<script setup lang="ts"></script>
<template>
  <v-sheet class="mx-auto" width="500">
    <v-form fast-fail @submit.prevent>
      <v-text-field
        variant="solo-filled"
        label="Имя исполнителя"
      ></v-text-field>

      <v-text-field
        variant="solo-filled"
        label="Название альбома"
      ></v-text-field>
      <v-text-field variant="solo-filled" label="Название песни"></v-text-field>
      <v-text-field variant="solo-filled" label="Жанр"></v-text-field>
      <v-text-field variant="solo-filled" label="Год выпуска"></v-text-field>
      <v-btn class="mt-2" type="submit" block>Редактировать</v-btn>
    </v-form>
  </v-sheet>
</template>
