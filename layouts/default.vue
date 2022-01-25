<template>
  <v-app>
    <v-card color="grey lighten-2" flat>
      <v-app-bar
        color="blue lighten-2"
        hide-on-scroll
        dark
        scroll-target="#scrolling-techniques-4"
      >
        <v-toolbar-title class="ml-4 font-weight-bold"
          >Todoリスト</v-toolbar-title
        >
      </v-app-bar>
    </v-card>
    <v-container>
      <Nuxt />
    </v-container>
    <v-footer>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
}
</script>
