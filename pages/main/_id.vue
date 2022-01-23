<template>
  <div>
    {{ showTasks }}
  </div>
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
