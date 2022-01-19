<template>
  <div>
    <div>ログイン</div>
    <v-form v-model="valid" lazy-validation
      ><v-row
        ><v-col cols="12" sm="6">
          <v-text-field
            v-model="name"
            :rules="[rules.required]"
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
          {{ valid }}
        </v-col>
      </v-row></v-form
    >
    <v-btn :disabled="!valid" @click="login">ログイン</v-btn>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',

  data() {
    return {
      // ユーザー名
      name: '',
      // メールアドレス
      email: '',
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
      // console.log('login')
      const res = await this.$axios.$post('http://localhost:8080/login/', {
        name: this.name,
        email: this.email,
      })
      //   console.log(res)
      if (res.length === 0) {
        //   console.log('failed')
        return
      }
      // ログインに成功したらメイン画面に遷移
      this.$router.push('/main')
    },
  },
}
</script>
