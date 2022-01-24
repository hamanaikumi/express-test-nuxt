<template>
  <v-container>
    <v-form v-model="valid" lazy-validation
      ><v-row justify="center" align-content="center">
        <v-col cols="12" sm="6" class="px-0 py-0 mt-10 mb-4">
          <v-text-field
            v-model="name"
            :rules="[rules.required]"
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
        @click="login"
        >ログイン</v-btn
      >
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'LoginPage',

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
     * ログインをする.
     */
    async login() {
      const res = await this.$axios.$post('http://localhost:8080/login/', {
        name: this.name,
        email: this.email,
      })
      if (res.length === 0) {
        return
      }
      // ログインに成功したらメイン画面に遷移
      this.$router.push({ path: `/main/${res[0].id}` })
    },
  },
}
</script>
