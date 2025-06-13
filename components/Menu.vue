<script setup>
const open = ref(false);
const fabLocation = ref("top right");
const menuLocation = ref("bottom center");
const transition = ref("scale-transition");

const router = useRouter();

function navigateTo(path) {
  router.push(path);
}
</script>
<template>
  <v-fab
    :absolute="true"
    :color="open ? '' : 'primary'"
    :location="fabLocation"
    size="large"
    icon
    class="ma-3"
  >
    <v-icon>{{ open ? "mdi-close" : "mdi-crown" }}</v-icon>
    <v-speed-dial
      v-model="open"
      :location="menuLocation"
      :transition="transition"
      activator="parent"
    >
      <v-btn key="1" color="success" icon @click="navigateTo('/adsfndf')">
        <v-icon size="24">mdi-check</v-icon>
      </v-btn>

      <v-btn key="2" color="info" icon @click="navigateTo('/adsfndf')">
        <v-icon size="24">mdi-information</v-icon>
      </v-btn>

      <v-btn key="3" color="warning" icon @click="navigateTo('/adsfndf')">
        <v-icon size="24">mdi-alert</v-icon>
      </v-btn>
      <!-- кнопка, которая закрывает speed dial -->
      <v-btn key="4" color="error" icon @click="open = false">
        <v-icon size="24">mdi-close-circle</v-icon>
      </v-btn>
    </v-speed-dial>
  </v-fab>
</template>
<style scoped>
h5 {
  margin-bottom: 12px;
}
</style>
