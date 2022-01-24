<template>
  <v-container>
    <v-row align-content="center" class="mt-1">
      <v-col cols="8">
        <v-text-field
          v-model="content"
          label="新しいタスク"
          outlined
          dense
        ></v-text-field>
      </v-col>
      <v-col>
        <v-btn depressed color="info" class="font-weight-bold" @click="addTask"
          >追加</v-btn
        >
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
export default {
  name: 'AddTaskPage',
  props: {
    categoryId: {
      type: String,
      required: true,
      default: '',
    },
  },
  data() {
    return {
      // タスク名
      content: '',
    }
  },
  methods: {
    /**
     * タスクを追加する.
     */
    async addTask() {
      const userId = Number(this.$route.params.id)
      await this.$axios.$post(
        `http://localhost:8080/categories/addTask/${userId}`,
        { category_id: this.categoryId, content: this.content }
      )
      // 画面を更新
      this.$emit('showTasks')
      // 初期化
      this.content = ''
    },
  },
}
</script>
