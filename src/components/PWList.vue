<template>
  <div>
    <v-subheader>リスト</v-subheader>
    <ul v-for="userAccount in accountData.$data.usersAccount" :key="userAccount.name">
      <v-list outlined>
        <v-list-item>
          <v-checkbox
              v-model="userAccount.checkedTarget"
          ></v-checkbox>
          <v-list-item-content>
            <v-list-item-title>{{ userAccount.name }}</v-list-item-title>
            <v-list-item-subtitle>{{ userAccount.email }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </ul>
    <div v-if = "accountData.$data.usersAccount.length !== 0">
      <slot name="del-btn" :deleteAccount="deleteAccount"></slot>
    </div>
  </div>
</template>

<script>
  import accountData from "../store/account-data.js";

  export default {
    name: "PWList.vue",
    data() {
      return {
        accountData,
      }
    },
    methods: {
      deleteAccount() {
        // API側で削除する
        alert('サーバーエンドで実装する機能');
      }
    },
  }
</script>