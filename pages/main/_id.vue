<template>
  <v-container>
    <AddCategory @showCategories="parentEvent" />
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
          <v-divider class="mx-4 mb-6"></v-divider>
          <v-card-text
            v-for="(task, j) of category.task"
            :key="j"
            class="py-0 text--primary text-body-1"
          >
            <v-row class="align-center">
              <v-col cols="1" class="mr-2">
                <input
                  type="checkbox"
                  :checked="task.taskCompleted !== '0'"
                  @change="complete(i, j)"
                />
              </v-col>
              <span v-show="task.taskCompleted === '0'">
                {{ task.taskContent }}
              </span>
              <span v-show="task.taskCompleted !== '0'" class="completed">
                {{ task.taskContent }}
              </span>
            </v-row>
          </v-card-text>
          <AddTask
            :category-id="category.categoryId.toLocaleString()"
            @showTasks="parentEvent"
          />
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
export default {
  name: 'MainPage',

  data() {
    return {
      // 表示用タスク一覧
      // eslint-disable-next-line no-array-constructor
      showTasks: Array<any>(),
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
    /**
     * タスクの完了/未完了を切り替える.(0:未、1:完)
     * @param i -showTask内のカテゴリー位置
     * @param j -showTask内のタスク位置
     */
    async complete(i: number, j: number) {
      const userId = Number(this.$route.params.id)
      const completeNum = this.showTasks[i].task[j].taskCompleted
      const taskId = this.showTasks[i].task[j].taskId
      if (completeNum === '0') {
        this.showTasks[i].task[j].taskCompleted = '1'
        await this.$axios.$post(
          `http://localhost:8080/categories/complete/${userId}`,
          {
            task_id: taskId,
            task_completed: '1',
          }
        )
      } else {
        this.showTasks[i].task[j].taskCompleted = '0'
        await this.$axios.$post(
          `http://localhost:8080/categories/complete/${userId}`,
          {
            task_id: taskId,
            task_completed: '0',
          }
        )
      }
    },
    /**
     * 子コンポーネントからのイベント発火
     */
    parentEvent() {
      this.getTasks()
    },
  },
}
</script>
<style scoped>
.completed {
  text-decoration: line-through;
  color: grey;
}
</style>
