<template>
  <section class="flex">
    <div>
      <img class="imag" src="~/assets/back_login.png" alt="" />
    </div>
    <v-container fluid>
      <v-row>
        <div class="h1">
          <h1>Login</h1>
        </div>
      </v-row>
      <v-row>
        <v-col align-self="center" cols="12" sm="6" md="4">
          <h3>Nome</h3>
        </v-col>
      </v-row>
      <v-row>
        <v-col align-self="center" cols="12" sm="10" md="10">
          <v-text-field
            class="email"
            outlined
            placeholder="Informe seu e-mail"
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col align-self="center" cols="12" sm="6" md="4">
          <h3>E-mail</h3>
        </v-col>
      </v-row>
      <v-row>
        <v-col align-self="center" cols="12" sm="10" md="10">
          <v-text-field
            class="email"
            outlined
            placeholder="Informe seu e-mail"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col align-self="center" cols="12" sm="6" md="4">
          <h3>Senha</h3>
        </v-col>
      </v-row>
      <v-row>
        <v-col align-self="center" cols="12" sm="10" md="10">
          <v-text-field
            v-model="password"
            :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
            :type="show2 ? 'text' : 'password'"
            name="input-10-1"
            outlined
            placeholder="Informe sua senha"
            @click:append="show2 = !show2"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col align-self="center" cols="12" sm="12" md="12">
          <h3>Confirme sua senha</h3>
        </v-col>
      </v-row>
      <v-row>
        <v-col align-self="center" cols="12" sm="10" md="10">
          <v-text-field
            v-model="password"
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :type="show1 ? 'text' : 'password'"
            name="input-10-1"
            outlined
            placeholder="Confirme sua senha"
            @click:append="show1 = !show1"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-container fluid class="pa-0">
        <div class="my-2">
        <v-btn large depressed color="#00995D" class="btnLogin"
          ><p class="entrar">
            <v-icon> mdi-account-check-outline </v-icon> Cadastrar
          </p></v-btn>
        </div>
        <div class="my-2">
        <v-btn large depressed to="~/" color="#00995D" class="btnLogin"
          ><p class="entrar"><v-icon> mdi-arrow-left </v-icon> Voltar</p>
        </v-btn>
        </div>
      </v-container>
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
    </v-container>
  </section>
</template>

<script lang="ts">
export default {
  layout: "auth",
  data: () => ({
    password: "",
    show1: false,
    show2: false,
  }),
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
  margin: 5px 0px 0px 5px;
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
  margin: 5% 35% 5% 35%;
}
.entrar {
  color: #f2f2f2;
}

.btnLogin {
 margin-top: 20px;
 min-width: 20%;
  width: 80%;
  
}
.imgLogo {
  margin: 5% 25% 2% 30%;
}
</style>