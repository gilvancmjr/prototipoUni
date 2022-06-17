<template>
  <section class="flex">
    <div>
      <img class="imag" src="~/assets/back_login.png" alt="" />
    </div>
    <div>
      <br />
      <v-row>
        <div class="h1">
          <h1>Login</h1>
        </div>
      </v-row>

      <v-row>
        <v-col cols="12" sm="6" md="4">
          <h3>E-mail</h3>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" sm="10" md="10">
          <v-text-field
            class="email"
            v-model="email"
            outlined
            placeholder="Informe seu e-mail*"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" sm="6" md="4">
          <h3>Senha</h3>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" sm="10" md="10">
          <v-text-field
            v-model="password"
            :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
            :type="show2 ? 'text' : 'password'"
            name="input-10-1"
            outlined
            placeholder="Informe sua senha*"
            @click:append="show2 = !show2"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-btn depressed color="#00995D" class="btnLogin" @click="acesso()"
          ><p class="entrar"><v-icon> mdi-login </v-icon> Entrar</p></v-btn
        >
      </v-row>
      <br />
      <v-row>
        <v-btn to="/auth/cadastro" depressed color="#00995D" class="btnLogin"
          ><p class="entrar">
            <v-icon> mdi-account-plus </v-icon> Não tenho Cadastro
          </p></v-btn
        >
      </v-row>
      <v-row>
        <v-col>
          <v-img
            class="imgLogo"
            max-height="60"
            max-width="200"
            contain
            :src="require('~/assets/logo_unimed.png')"
          />
        </v-col>
      </v-row>
    </div>
  </section>
</template>

<script>
import Axios from "axios";
export default {
  layout: "auth",
  data() {
    return {
      password: "",
      email: "",
      show2: false,
    };
  },
  methods: {
    acesso() {
      Axios.get('http://localhost:3000/users?email=' + this.email +'&senha=' + this.password).then((response) => {
        console.log(response.data[0])
        localStorage.setItem('permissao', response.data[0].permissao)
        this.$router.replace('/informacao')
      });


    },
  },
};
</script>

<style>
.containe {
  display: inline-block;
  flex-direction: column;
  align-items: center;
  height: 100vh;
}

.flex {
  display: grid;
  align-items: top;
  grid-template-columns: 1fr 1fr;
  grid-grap: 20px;
}
.imag {
  max-width: 700px;
  width: 100%;
  max-height: 1024px;
  height: 100%;
  background-size: 100%;
}

.logo {
  position: absolute;
  width: 720px;
  height: 1024px;
  left: 0px;
  top: 0px;

  background: #f2f2f2;
}
.h1 {
  font-size: 30px;
  margin: 15% 35% 15% 35%;
}
.entrar {
  color: #f2f2f2;
}

.btnLogin {
  width: 30%;
  height: 90px;

  margin: 2% 25% 2% 25%;
}
.imgLogo {
  margin: 12% 25% 2% 30%;
}
</style>