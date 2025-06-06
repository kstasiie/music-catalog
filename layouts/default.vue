<template>
  <v-app>
    <v-main class="main-container">
      <slot />
    </v-main>
  </v-app>
</template>
<style scoped lang="scss">
.main-container {
  background: linear-gradient(to bottom, #87ceeb, #1e90ff) !important;
  min-height: 100vh;
}
</style>