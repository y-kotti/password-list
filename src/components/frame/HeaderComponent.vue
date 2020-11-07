<template>
  <v-app-bar app>
    <!-- アプリケーションバー -->
    <v-app-bar-nav-icon @click.stop="appData.$data.drawer = !appData.$data.drawer"/>
    {{ app_name }}
    <v-spacer></v-spacer>
  </v-app-bar>
</template>

<script>
  import appData from "@/store/app-data.js";

  export default {
    name: "HeaderComponent.vue",
    data() {
      return {
        appData,
        app_name: 'PW管理ツール',
        search: [{form: '入力してください', icon: 'mdi-magnify'}],
        search_text: '',
      }
    }
  }
</script>