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
// import index from '~/pages/index.vue'
// import login from '~/pages/login.vue'
// import main from '~/pages/main.vue'
export default {
  // components: { index,login },
  name: 'DefaultLayout',
  data() {
    return {
      fixed: false,
    }
  },
}
</script>
