<script setup>
import { ref } from "vue";

const open = ref(false);
const fabLocation = ref("top right");
const menuLocation = ref("bottom center");
const transition = ref("scale-transition");
</script>
<template>
  <v-fab
    :absolute="true"
    :color="open ? '' : 'primary'"
    :location="fabLocation"
    size="large"
    icon
  >
    <v-icon>{{ open ? "mdi-close" : "mdi-crown" }}</v-icon>
    <v-speed-dial
      v-model="open"
      :location="menuLocation"
      :transition="transition"
      activator="parent"
    >
      <v-btn key="1" color="success" icon>
        <v-icon size="24">mdi-check</v-icon>
      </v-btn>

      <v-btn key="2" color="info" icon>
        <v-icon size="24">mdi-information</v-icon>
      </v-btn>

      <v-btn key="3" color="warning" icon>
        <v-icon size="24">mdi-alert</v-icon>
      </v-btn>

      <v-btn key="4" color="error" icon>
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
