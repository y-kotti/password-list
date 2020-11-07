<template>
  <v-container>
    <v-row>
      <v-col>
        <PWEdit></PWEdit>
      </v-col>
      <v-divider vertical></v-divider>
      <v-col>
        <PWList>
          <template v-slot:del-btn="{ deleteAccount }">
            <v-btn @click="deleteAccount" class="float-right">
              delete
            </v-btn>
          </template>
        </PWList>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import PWList from '../components/PWList.vue';
  import PWEdit from '../components/PWEdit.vue';

  export default {
    name: "Home.vue",
    data() {
      return {
        accountData: [],
      }
    },
    components: {
      PWList,
      PWEdit,
    },
  }
</script>
