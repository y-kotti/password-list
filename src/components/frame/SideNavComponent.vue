<template>
  <v-navigation-drawer app :dark="appData.$data.darkMode" v-model="appData.$data.drawer">
    <v-list-item>
      <v-list-item-title class="title">
        Menu
      </v-list-item-title>
    </v-list-item>
    <v-divider/>
    <v-list nav>
      <v-list-item v-for="menu in menus" :key="menu.title" :to="menu.url">
        <v-list-item-icon>
          <v-icon>{{ menu.icon }}</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ menu.title }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
  import appData from "@/store/app-data.js";

  export default {
    name: "SideNavComponent.vue",
    data() {
      return {
        appData,
        search: [{form: '入力してください', icon: 'mdi-magnify'}],
        menus: [
          {title: 'Home', icon: 'mdi-home', url: '/'},
          {title: '設定', icon: 'mdi-cog', url: '/setting'},
        ],
        dark: false
      }
    }
  }
</script>