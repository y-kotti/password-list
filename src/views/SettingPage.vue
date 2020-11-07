<template>
  <v-container>
    <v-row>
      <v-col>
          <v-switch
              v-model="appData.$data.darkMode"
              :label="`DarkMode`"
          ></v-switch>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import appData from "@/store/app-data.js";

  export default {
    name: "SettingPage.vue",
    data() {
      return {
        appData,
      }
    }
  }
</script>

<style scoped>

</style>