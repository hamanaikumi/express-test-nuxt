<template>
  <v-app>
    <v-card color="grey lighten-4" flat>
      <v-toolbar dense>
        <v-toolbar-title>Todoリスト</v-toolbar-title>
      </v-toolbar>
    </v-card>
    <v-container>
      <Nuxt />
    </v-container>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import index from '~/pages/index.vue'
export default {
  components: { index },
  name: 'DefaultLayout',
  data() {
    return {
      fixed: false,
    }
  },
}
</script>
