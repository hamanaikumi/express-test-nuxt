<template>
  <v-container>
    <v-btn class="ma-2" text icon color="blue lighten-2" @click="deleteTask">
      <v-icon> mdi-trash-can-outline </v-icon>
    </v-btn>
  </v-container>
</template>

<script lang="ts">
export default {
  name: 'DeleteTaskPage',
  props: {
    taskId: {
      type: String,
      required: true,
      default: '',
    },
  },
  methods: {
    /**
     * タスクを削除する.
     */
    async deleteTask() {
      const userId = Number(this.$route.params.id)
      await this.$axios.$post(
        `http://localhost:8080/categories/deleteTask/${userId}`,
        { task_id: this.taskId }
      )
      // 画面を更新
      this.$emit('showTasks')
    },
  },
}
</script>
