<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-img
      style="margin-bottom: 50px;"
        class="logo"
        max-height="60"
        max-width="200"
        contain
        :src="require('~/assets/logo_unimed.png')"
      />
      <br />
      <div>
      <v-list>
        <div>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <br />
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
        </div>
      </v-list>
      </div>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

      <v-toolbar-title><h1>Bem vindo</h1></v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-email</v-icon>
      </v-btn>

      <v-img
        max-height="30"
        max-width="30"
        contain
        :src="require('~/assets/perfil.png')"
      />
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" :right="right" temporary fixed>
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light> mdi-repeat </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  created() {
    this.permissao = localStorage.getItem('permissao');
    this.gerarMenu()
     console.log(localStorage.getItem('permissao'))
  },
  methods: {
    gerarMenu() {
      if (this.permissao === "Administrador") {
        this.items = [
          {
            icon: "mdi-home",
            title: "Home",
            to: "/informacao",
          },
          {
            icon: "mdi-account",
            title: "Usuário",
            to: "/user/cadastro-user-uni",
          },
          {
            icon: " mdi-exit-to-app",
            title: "Sair",
            to: "/",
          },
        ];
      } else if (this.permissao === "Usuario") {
        this.items = [
          {
            icon: "mdi-home",
            title: "Home",
            to: "/informacao",
          },
          {
            icon: " mdi-exit-to-app",
            title: "Sair",
            to: "/",
          },
        ];
      }
    },
  },
  data() {
    return {
      permissao: "",
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Vuetify.js",
      clipped: false,
      drawer: false,
      fixed: false,
      items: [],
    };
  },
};
</script>

<style>
.logo {
  margin: 10%;
}
</style>
