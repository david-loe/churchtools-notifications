<template>
  <div class="text-center" id="loginPage">
    <form class="form-signin" @submit.prevent="login()">
      <i class="bi bi-bell" style="font-size: 8rem"></i>
      <h1 class="h3 mb-3 fw-normal">{{ $t('labels.signIn') }}</h1>

      <div class="form-floating">
        <input type="email" class="form-control" id="email" placeholder="name@example.com" v-model="email" required />
        <label for="email">{{ $t('labels.email') }}</label>
      </div>
      <div class="form-floating">
        <input
          type="password"
          class="form-control"
          id="floatingPassword"
          placeholder="{{ $t('labels.password') }}"
          v-model="password"
          autocomplete="current-password"
          required
        />
        <label for="floatingPassword">{{ $t('labels.password') }}</label>
      </div>

      <button class="w-100 btn btn-lg btn-primary" type="submit">{{$t('labels.signIn')}}</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Login',
  data() {
    return {
      password: '',
      email: '',
    }
  },
  methods: {
    async login() {
      try {
        const res = await axios.post(
          process.env.VUE_APP_BACKEND_URL + '/ct/login',
          {
            username: this.email,
            password: this.password,
            rememberMe: true,
          },
          { withCredentials: true },
        )
        if (res.status === 200) {
          this.$root.loadState = 'UNLOADED'
          this.$router.push('/')
        }
      } catch (error) {
        this.password = ''
        alert(this.$t('alerts.loginFailed'))
      }
    },
  },
  beforeMount() {
    this.$root.loadState = 'LOADED'
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.loginPage {
  height: 100%;
  display: flex;
  align-items: center;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}

.form-signin .checkbox {
  font-weight: 400;
}

.form-signin .form-floating:focus-within {
  z-index: 2;
}

.form-signin input[type='email'] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}

.form-signin input[type='password'] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
