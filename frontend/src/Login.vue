<template>
  <q-layout class="vertical-center">
    <q-page-container>
      <q-page class="flex justify-end items-center">
        <q-ajax-bar
          position="top"
          color="primary"
          size="5px"
        />
        <q-card
          bordered
          class="card q-pa-md shadow-10"
          style=" background-color: #101010; height:100%; border-color: transparent;  "
        >
          <q-card-section class="text-primary text-center">
            <q-img
              src="../assets/logo-ofical.png"
              spinner-color="white"
              style="height: 111.54px; max-width: 300px"
              class="q-mb-lg q-px-md"
            />
          </q-card-section>
          <q-card-section class="text-primary">
            <div class="text-h6-login">Bem vindo!</div>
            <div class="text-caption-login"> Faça Login...</div>
          </q-card-section>

          <q-card-section >
            <q-input
              style="color: #fff !important;"
              class="q-mb-md "
              clearable
              rounded
              v-model="form.email"
              placeholder="seuemail@provedor.com"
              @blur="$v.form.email.$touch"
              :error="$v.form.email.$error"
              error-message="Deve ser um e-mail válido."
              outlined
              @keypress.enter="fazerLogin"
            >
              <template v-slot:prepend  >
                <q-icon
                  style="color: #D9D9D9; font-color:#fff  "
                  name="mdi-email-outline"
                  class="cursor-pointer"
                  color='primary'
                />
              </template>
            </q-input>

            <q-input
             class="cor-input"
              outlined
              rounded
              placeholder="Digite sua senha"
              v-model="form.password"
              :type="isPwd ? 'password' : 'text'"
              @keypress.enter="fazerLogin"
            >
              <template v-slot:prepend>
                <q-icon
                  name="mdi-shield-key-outline"
                  class="cursor-pointer"
                  color='primary'
                />
              </template>
              <template v-slot:append>
                <q-icon
                  :name="isPwd ? 'visibility_off' : 'visibility'"
                  class="cursor-pointer"
                  @click="isPwd = !isPwd"
                />
              </template>
            </q-input>
          </q-card-section>
          <q-card-actions>
            <q-space />
            <q-btn
              class="q-mr-sm q-my-lg"
              style="width: 100%;padding-top:5px;padding-bottom:5px; font-family:'Poppins', sans-serif; font-size: 16px; font-weight: 700;"
              color="primary"
              rounded
              :loading="loading"
              @click="fazerLogin"
            >
              Login
              <span slot="loading">
                <q-spinner-puff class="on-left" />Logando...
              </span>
            </q-btn>
          </q-card-actions>
          <!-- <q-btn
            flat
            color="info"
            no-caps
            dense
            class="q-px-sm"
            label="Esqueci a senha"
            @click="modalEsqueciSenha=true"
          /> -->

          <q-inner-loading :showing="loading" />
        </q-card>
      </q-page>

    </q-page-container>
  </q-layout>
</template>

<script>
import { required, email } from 'vuelidate/lib/validators'

export default {
  name: 'Login',
  data () {
    return {
      modalEsqueciSenha: false,
      emailRedefinicao: null,
      form: {
        email: null,
        password: null
      },
      contasCliente: {},
      isPwd: true,
      loading: false
    }
  },
  validations: {
    form: {
      email: { required, email },
      password: { required }
    },
    emailRedefinicao: { required, email }
  },
  methods: {
    fazerLogin () {
      this.$v.form.$touch()
      if (this.$v.form.$error) {
        this.$q.notify('Informe usuário e senha corretamente.')
        return
      }
      this.loading = true
      this.$store.dispatch('UserLogin', this.form)
        .then(data => {
          // if (Object.keys(this.contasCliente).length == 1) {
          //   // logar direto
          // }
          this.loading = false
          // .params = { modalEscolhaUnidadeNegocio: true }
        })
        .catch(err => {
          console.error('exStore', err)
          this.loading = false
        })
    },
    clear () {
      this.form.email = ''
      this.form.password = ''
      this.$v.form.$reset()
    }
  },
  mounted () {
  }
}
</script>
<style scoped>

.q-field__native{
  color: #fff;
}
.cor-input{
  color:#FFF;
}
.input-color{
  color: #FFF !important;
}
#login-app {
  background: none;
}
.form-login{
  color: #FFFFFF important;
  border-color: #FFFFFF ;
}
.text-h6-login{
  font-size: 40px;
  color: #FFFFFF;
  font-family: 'Poppins', sans-serif;
  font-weight: 700;
  text-align: center;
  margin-top: -50px;
}
.text-caption-login {
  font-size: 20px;
  opacity: 0.5;
  color: #FFFFFF;
  font-family: 'Poppins', sans-serif;
  font-weight: 700;
  text-align: center;
}

.index {
  width: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  text-align: center;
  background-repeat: no-repeat;
  background-size: cover;
  overflow: hidden;
}

.index h1 {
  height: 150px;
}

.index h1 img {
  height: 100%;
}

.index h2 {
  color: #666;
  margin-bottom: 200px;
}

.index h2 p {
  margin: 0 0 50px;
}

q-input {
  color: #fff!important;
}

.index .ivu-row-flex {
  height: 100%;
}

#indexLizi {
  position: absolute;
  width: 100%;
  top: 0;
  bottom: 0;
  overflow: hidden;
}

.bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.card {
  width: 100%;
  max-width: 375px;
  margin-right: 10%;
}

.q-img__image {
  background-repeat: no-repeat;
  background-size: contain;
}
</style>
