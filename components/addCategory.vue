<template>
  <div>
    <!-- ボタン押下でモーダルダイアログを開きます。 -->
    <v-row class="my-8 ml-8">
      <v-btn
        depressed
        color="info"
        class="font-weight-bold"
        @click.stop="dialog = true"
        >カテゴリーを追加</v-btn
      >
    </v-row>
    <!-- モーダルダイアログを定義します。persistentを指定するとモーダルになります。 -->
    <v-dialog v-model="dialog" max-width="400" persistent>
      <v-card class="py-8">
        <v-container>
          <v-row justify="center">
            <v-col cols="8">
              <v-text-field
                v-model="categoryName"
                label="新しいカテゴリー"
                outlined
              ></v-text-field>
            </v-col>
          </v-row>
          <v-card-actions>
            <v-row justify="center">
              <v-btn class="mx-2" @click="dialog = false">閉じる</v-btn>
              <v-btn class="mx-2" @click="addCategory">保存</v-btn>
            </v-row>
          </v-card-actions>
        </v-container>
      </v-card>
    </v-dialog>
  </div>
</template>

<script lang="ts">
export default {
  name: 'AddCategoryPage',
  data() {
    return {
      // ダイアログの開く/閉じるを制御します。
      dialog: false,
      // カテゴリー名
      categoryName: '',
    }
  },
  methods: {
    /**
     * カテゴリーを追加する.
     */
    async addCategory() {
      const userId = Number(this.$route.params.id)
      await this.$axios.$post(
        `http://localhost:8080/categories/addCategory/${userId}`,
        { category_name: this.categoryName }
      )
      // 画面を更新
      this.$emit('showCategories')
      // 初期化
      this.categoryName = ''
      this.dialog = false
    },
  },
}
</script>
