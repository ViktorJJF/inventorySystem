<template>
  <v-app-bar dark app color="primary">
    <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
    <span class="title ml-3 mr-5">Fulltec</span>
    <v-spacer></v-spacer>
    <v-menu v-if="$store.state.user" offset-y>
      <template v-slot:activator="{ on }">
        <v-btn icon v-on="on">
          <v-badge color="red" @click>
            <template v-slot:badge>{{$store.state.stockAlert.length}}</template>
            <v-icon>mdi-email</v-icon>
          </v-badge>
        </v-btn>
      </template>
      <v-list v-for="(alertMessage,i) in $store.state.stockAlert" :key="i">
        <v-list-item>
          Tu producto {{alertMessage.productName}} está por agortarse. (
          <strong>Stock:</strong>
          {{alertMessage.stock}})
        </v-list-item>
      </v-list>
    </v-menu>
    <v-btn v-if="!$store.state.user" dark outlined :to="{name:'login'}">Iniciar Sesión</v-btn>
    <v-menu v-if="$store.state.user" offset-y>
      <template v-slot:activator="{ on }">
        <v-btn text color="white" dark v-on="on">
          {{user}}
          <v-icon>mdi-menu-down</v-icon>
        </v-btn>
      </template>
      <v-list>
        <v-list-item @click="logout">
          <v-list-item-title>Cerrar sesión</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-app-bar>
</template>

<script>
export default {
  computed: {
    drawer: {
      get() {
        return this.$store.state.toolbar.drawerIcon;
      },
      set(newValue) {
        this.$store.state.toolbar.drawerIcon = newValue;
      }
    },
    user() {
      return this.$store.state.user.email;
    }
  },
  methods: {
    logout() {
      this.$store
        .dispatch("logout")
        .then(res => {
          console.log("se pusheara el login");
          this.$router.push({ name: "login" });
        })
        .catch(err => {
          console.log("algo salio mal en logout");
        });
    }
  }
};
</script>

<style lang="scss" scoped>
</style>