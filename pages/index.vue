<template>
  <div>
    <div>ユーザー登録</div>
    <v-row
      ><v-col cols="12" sm="6">
        <v-text-field
          v-model="name"
          :rules="[rules.required, rules.counter]"
          label="ユーザー名を入力"
          outlined
      /></v-col>
      <v-col cols="12" sm="6">
        <v-text-field
          v-model="email"
          :rules="[rules.required, rules.email]"
          label="メールアドレスを入力"
          outlined
        />
      </v-col>
    </v-row>
    <v-btn>登録</v-btn>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      name: '',
      email: '',
      rules: {
        required: (value) => !!value || '入力してください.',
        counter: (value) =>
          value.length <= 20 || '20文字以内で入力してください',
        email: (value) => {
          const pattern =
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(value) || '無効なメールアドレスです'
        },
      },
    }
  },
}
</script>
