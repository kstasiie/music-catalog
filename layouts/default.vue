<template>
  <v-app>
    <Menu />
    <v-main class="main-container">
      <slot />
    </v-main>
  </v-app>
</template>
<style lang="scss">
.v-application {
  background: linear-gradient(to bottom, #87ceeb, #1e90ff) !important;
}
</style>
