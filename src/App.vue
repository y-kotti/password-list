<template>
  <v-app>
    <SideNavComponent></SideNavComponent>
    <HeaderComponent></HeaderComponent>
    <v-content>
      <router-view/>
    </v-content>
  </v-app>
</template>

<script>
  import HeaderComponent from './components/frame/HeaderComponent.vue';
  import SideNavComponent from './components/frame/SideNavComponent.vue';

  export default {
    name: 'App',
    components: {
      HeaderComponent,
      SideNavComponent,
    },
  }
</script>