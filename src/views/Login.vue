<template>

  <div class="reg_log">
    <router-link tag=btn to="/login">
        <v-btn class="white--text"
            depressed
            tile
            width="280px"
            height="55px"
            color="red">
                Masuk
        </v-btn>
    </router-link>
    <router-link tag=btn to="/register">
        <v-btn class="red--text"
            depressed
            tile
            width="280px"
            height="55px">
                Daftar
        </v-btn>
    </router-link>

    <v-row 
        no-gutters
        class="spacing-playground pa-10"
        fluid>
        <v-col
            cols="12"
            md="4"
        >
            <v-col md="20">
                <v-row>
                    <v-col md="20">
                        <v-img
                            :src="require('../assets/logologin.png')"
                            contain
                            height="75"
                        />
                        <br />
                        <v-form 
                            class="mt-5"
                            ref="form"
                            v-model="valid"
                            lazy-validation
                        >
                            <p class="font-weight-bolt">EMAIL</p>
                            <v-text-field
                                v-model="param.email"
                                :rules="emailRules"
                                label=""
                                clearable
                                required
                                color="red"
                            ></v-text-field>

                            <p></p>
                            <p class="font-weight-bolt">PASSWORD</p>
                            <v-text-field
                                v-model="param.password"
                                :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
                                :type="show ? 'text' : 'password'"
                                label=""
                                clearable
                                required
                                color="red"
                                @click:append="show = !show"
                                :rules="passwordRules"
                            ></v-text-field>

                            <v-checkbox
                                v-model="checkbox"
                                :rules="ingat"
                                label="INGAT SAYA"
                                required
                                color="red"
                            ></v-checkbox>

                            <p></p>

                            <v-btn
                                rounded
                                width="229px"
                                height="40px"
                                color="red"
                                class="d-flex align-center justify-center pa-4 mx-auto white--text"
                                @click="login()"
                                :disabled="!valid"
                            >
                                Masuk
                            </v-btn>

                        </v-form>
                    </v-col>
                </v-row>
            </v-col>
      </v-col>
    </v-row>
    </div>
</template>

<style scoped>
.reg_log {
  background: url('../assets/background2.jpeg');
  background-size: cover;
  height: 100vh;
}
</style>

<script>
import {Services} from '../services/Services'
const APIServices = new Services()
export default {
    name: "Login",
    data() {
        return {
            show: false,
            param: {
                email: '',
                password: '',
            },
            emailRules: [
                v => !!v || 'Email is required',
            ],
            passwordRules: [
                v => !!v || 'Password is required',
            ],
        }
    },
    methods: {
        async login() {
            const data = await APIServices.login(this.param).then((succ) => succ)
            console.log(data)
            if (data.code === 1) {
                var session = {
                    email: data.body.email,
                    password: data.body.password,
                    name: data.body.name,
                    username: data.body.username,
                }
                this.$swal({
                    icon: 'success',
                    title: 'Success',
                    text: data.message,
                    type: 'success'
                }).then((result) => {
                    if (result.isConfirmed){
                        this.$session.start()
                        this.$session.set('loginStat', true)
                        this.$session.set('dataUser', session)
                        this.$router.push('/')
                    }
                })
            } else {
                console.log(data)
                this.$swal({
                    icon: 'error',
                    title: 'Error',
                    text: data.message,
                    type: 'error'
                });
            }
            }
        }
}
</script>