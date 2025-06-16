<script setup lang="ts">
import { ref } from "vue";

const emailAddress = ref("music-catalog-help-service@yandex.ru");
const copiedMessage = ref("");

const copyEmail = async () => {
  try {
    await navigator.clipboard.writeText(emailAddress.value);
    copiedMessage.value = "Адрес скопирован!";
    setTimeout(() => {
      copiedMessage.value = "";
    }, 2000); // Сообщение исчезнет через 2 секунды
  } catch (err) {
    console.error("Не удалось скопировать текст: ", err);
    copiedMessage.value = "Ошибка при копировании.";
  }
};
</script>

<template>
  <v-container class="text-center">
    <img src="~/assets/images/main-logo.svg" alt="" />

    <v-row class="d-flex justify-center">
      <v-col cols="12" sm="9" lg="6" xl="4">
        <v-btn
          rounded
          class="mt-4"
          type="submit"
          block
          color="primary"
          href="https://limewire.com/d/Dt6mb#BrSKLeDFJi"
          download
        >
          Руководство пользователя
        </v-btn>

        <v-btn
          rounded
          class="mt-4"
          type="submit"
          block
          color="primary"
          @click="copyEmail"
          >Почта службы поддержки
        </v-btn>
        <span v-if="copiedMessage" class="ml-2 success--text">{{
          copiedMessage
        }}</span>
      </v-col>
    </v-row>
  </v-container>
</template>
