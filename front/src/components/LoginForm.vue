<template>
  <div>
    <h2>ログイン</h2>
    <!-- ======= 👇 ここから変更する ======= -->
    <form @submit.prevent="login">
      <!-- ====== 👆 ここまで変更する ======= -->
      <input type="email" required placeholder="メールアドレス" v-model="email">
      <input type="password" required placeholder="パスワード" v-model="password">
      <!-- ======= 👇 ここから追加する ======= -->
      <div class="error">{{ error }}</div>
      <!-- ====== 👆 ここまで追加する ======= -->
      <button>ログインする</button>
    </form>
  </div>
</template>

<!-- ... 省略 -->
<script>
import axios from 'axios'
// ======= 👇 ここから追加する =======
import setItem from '../auth/setItem'
// ====== 👆 ここまで追加する =======

export default {
  emits: ['redirectToChatRoom'],
  data () {
    return {
      email: '',
      password: '',
      error: null
    }
  },
  methods: {
    async login() {
      try {
        this.error = null
        const res = await axios.post('http://localhost:3000/auth/sign_in', {
          email: this.email,
          password: this.password,
          }
        )
        if (!res) {
          throw new Error('メールアドレスかパスワードが違います')
        }

        if (!this.error) {
        // ======= 👇 ここから変更する =======
          setItem(res.headers, res.data.data.name)
        // ====== 👆 ここまで変更する =======
          this.$emit('redirectToChatRoom')
        }

        console.log({ res })
        return res
      } catch (error) {
        console.log({ error })
        this.error = 'メールアドレスかパスワードが違います'
      }
    }
  }
}
</script>
