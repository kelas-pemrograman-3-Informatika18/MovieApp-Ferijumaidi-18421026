<template>
    <q-layout class="bg-blue-grey-2" view="lHh Lpr Lff">
      <q-page-container>
        <q-page padding class="row items-center justify-center">
            <div class="full-width row">
              <div class="col-md-6 offset-md-3 col-xs-12 q-pa-md">
                  <q-card flat class="text-blue-grey-9">
                      <div class="row">
                          <div class="col-md-6">
                            <div class="row">
                                <div class="col-md-8 offset-2">
                                    <q-img src="loginn.PNG"></q-img>
                                </div>
                            </div>
                          </div>
                          <div class="col md-6">
                            <q-card-section>
                              <div class="text-h5">Movie App</div>
                              <div>Masukan Akun</div>
                            </q-card-section>
                              <q-form @submit="login">
                                <q-card-section>
                                    <q-input
                                      v-model="username"
                                      label="Username"
                                      :rules="[
                                      val => val && val.length > 0 || 'Masukan Username'
                                      ]"
                                    />
                                    <q-input
                                      v-model="password"
                                      label="Password"
                                      type="password"
                                      :rules="[
                                      val => val && val.length > 0 || 'Masukan Password'
                                      ]"
                                    />
                                </q-card-section>
                                <q-card-section>
                                    <q-btn label="Login" type="submit" unelevated color="primary" class="full-width" />
                                    <q-btn :to="{ name: 'registerPage' }" label="Register" class="full-width q-mb-md q-mt-md"/>
                                </q-card-section>
                             </q-form>
                          </div>
                        </div>
                  </q-card>
              </div>
            </div>
        </q-page>
      </q-page-container>
    </q-layout>
</template>
<script>
export default {
  data () {
    return {
      username: null,
      password: null
    }
  },
  methods: {
    login () {
      this.$axios.post('user/login', {
        username: this.username,
        password: this.password
      }).then(res => {
        if (res.data.sukses) {
          this.$q.localStorage.set('dataUser', res.data.data)
          if (res.data.data.level === 1) {
            this.$router.push({ name: 'dashboardAdmin' })
          } else {
            this.$router.push({ name: 'userPage' })
          }
        } else {
          this.$showNotif(res.data.pesan, 'negative')
        }
      })
    }

  }
}
</script>
