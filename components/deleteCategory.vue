<template>
  <v-container>
    <v-btn class="ma-2" text icon color="red lighten-2" @click="deleteCategory">
      <v-icon> mdi-close-circle-outline </v-icon>
    </v-btn>
  </v-container>
</template>

<script lang="ts">
export default {
  name: 'DeleteCategoryPage',
  props: {
    // カテゴリーID
    categoryId: {
      type: String,
      required: true,
      default: '',
    },
  },
  methods: {
    /**
     * カテゴリーを削除する.
     */
    async deleteCategory() {
      const userId = Number(this.$route.params.id)
      await this.$axios.$post(
        `http://localhost:8080/categories/deleteCategory/${userId}`,
        { category_id: this.categoryId }
      )
      // 画面を更新
      this.$emit('showTasks')
    },
  },
}
</script>
