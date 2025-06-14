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
    size="x-large"
    icon
    class="ma-3"
  >
    <v-icon>{{ open ? "mdi-close" : "mdi-menu" }}</v-icon>
    <v-speed-dial
      v-model="open"
      :location="menuLocation"
      :transition="transition"
      activator="parent"
    >
      <v-btn key="1" color="info" icon @click="navigateTo('/')">
        <v-icon size="30">mdi-magnify</v-icon>
      </v-btn>

      <v-btn key="2" color="info" icon @click="navigateTo('/add-track')">
        <v-icon size="24">mdi-plus</v-icon>
      </v-btn>

      <v-btn key="3" color="info" icon @click="navigateTo('/print-tracks')">
        <v-icon size="24">mdi-music-box</v-icon>
      </v-btn>
      <!-- кнопка, которая закрывает speed dial -->
      <v-btn key="4" color="info" icon @click="navigateTo('/adsfndf')">
        <v-icon size="24">mdi-information</v-icon>
      </v-btn>
    </v-speed-dial>
  </v-fab>
</template>
<style scoped>
h5 {
  margin-bottom: 12px;
}
</style>
