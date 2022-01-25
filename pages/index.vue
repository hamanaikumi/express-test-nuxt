<template>
  <v-container>
    <v-row justify="center">
      <div class="text-h6 mt-4 font-weight-bold">ユーザー登録</div>
    </v-row>
    <v-form v-model="valid" lazy-validation>
      <v-row justify="center" align-content="center"
        ><v-col cols="12" sm="6" class="px-0 py-0 mt-10 mb-4">
          <v-text-field
            v-model="name"
            :rules="[rules.required, rules.counter]"
            label="ユーザー名を入力"
            outlined
          />
          <v-text-field
            v-model="email"
            :rules="[rules.required, rules.email]"
            label="メールアドレスを入力"
            outlined
          />
        </v-col>
      </v-row>
    </v-form>
    <v-row justify="center">
      <v-btn
        depressed
        color="info"
        class="font-weight-bold"
        :disabled="!valid"
        @click="register"
        >登録</v-btn
      >
    </v-row>
    <v-row justify="center">
      <div class="mt-4">
        登録済みの方はこちらから <router-link to="/login">ログイン</router-link>
      </div>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'SignupPage',
  data() {
    return {
      // ユーザー名
      name: '',
      // メールアドレス
      email: '',
      //   入力値の有効性チェック
      valid: false,
      // バリデーションチェック用
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
  methods: {
    /**
     * ユーザー登録をする.
     */
    async register() {
      const res = await this.$axios.$post('http://localhost:8080/register/', {
        name: this.name,
        email: this.email,
      })
      if (res.length === 0) {
        return
      }
      // 登録に成功したらログイン画面に遷移
      this.$router.push('/login')
    },
  },
}
</script>
