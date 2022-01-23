<template>
  <v-container>
    <v-row>
      <v-col
        v-for="(category, i) of showTasks"
        :key="i"
        class="d-flex"
        cols="12"
        sm="6"
        md="6"
        lg="4"
      >
        <v-card class="ml-8 mt-4 pa-2">
          <!-- Category -->
          <v-row class="align-center mx-0 d-flex justify-space-between">
            <v-card-title>
              {{ category.categoryName }}
            </v-card-title>
            <span>
              <!-- DeleteCategory -->
            </span>
          </v-row>
          <v-divider class="mx-4 mb-2"></v-divider>
          <v-card-text
            v-for="(task, j) of category.task"
            :key="j"
            class="py-0 text--primary text-body-1"
          >
            <v-row class="align-center">
              <v-col cols="1" class="mr-2">
                <input type="checkbox" />
              </v-col>
              <span v-show="task.taskCompleted === '0'">
                {{ task.taskContent }}
              </span>
              <span v-show="task.taskCompleted !== '0'" class="completed">
                {{ task.taskContent }}
              </span>
              <span>
                <!-- Add Tasks -->
              </span>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'MainPage',

  data() {
    return {
      // 表示用タスク一覧
      showTasks: Array({}),
    }
  },
  created() {
    this.getTasks()
  },
  methods: {
    /**
     * ユーザーIDに一致するタスク一覧を表示する.
     */
    async getTasks() {
      // 初期化
      this.showTasks = []
      const userId = Number(this.$route.params.id)
      const res = await this.$axios.$get(
        `http://localhost:8080/categories/${userId}`
      )
      for (const task of res) {
        const taskIdArray = task.task_id ? task.task_id.split(',') : ''
        const taskContentArray = task.task_content
          ? task.task_content.split(',')
          : ''
        const taskCompleteArray = task.task_completed
          ? task.task_completed.split(',')
          : ''

        const taskArray = []
        for (let i = 0; i < taskIdArray.length; i++) {
          taskArray.push({
            taskId: taskIdArray[i],
            taskContent: taskContentArray[i],
            taskCompleted: taskCompleteArray[i],
          })
        }
        this.showTasks.push({
          categoryId: task.id,
          categoryName: task.category_name,
          task: taskArray,
        })
      }
    },
  },
}
</script>
