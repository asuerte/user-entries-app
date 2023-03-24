<template>
  <v-app>
    <v-app-bar elevation="1" color="yellow-lighten-4">
      <v-app-bar-title class="text-center text-black text-h4">
        U<span>SER ENTRIES</span>
      </v-app-bar-title>
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row>
          <v-col>
            <NewUserForm
              @addNewUser="handleNewUser"
              @updateExistingUser="updateExistingUser"
              :selectedUser="selectedUser"
              :isEditMode="isEditMode"
            ></NewUserForm>
          </v-col>
          <v-col v-if="userList.length > 0">
            <UserEntries
              :userList="userList"
              @emitRemoveUser="removeUserEntry"
              @emitUpdateUser="setEditMode"
            ></UserEntries>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import NewUserForm from "@/components/NewUserForm.vue";
import UserEntries from "@/components/UserEntries.vue";

export default {
  name: "App",
  components: { NewUserForm, UserEntries },
  data() {
    return {
      userList: [],
      selectedUser: undefined,
      isEditMode: false,
    };
  },
  methods: {
    handleNewUser(newUserItem) {
      this.userList.push(newUserItem);
    },
    removeUserEntry(userId) {
      this.userList = this.userList.filter((user) => user.id !== userId);
    },
    setEditMode(user) {
      this.selectedUser = user;
      this.isEditMode = true;
    },
    updateExistingUser(userInfo) {
      let index = this.userList.findIndex((user) => user.id === userInfo.id);
      this.userList[index] = userInfo;
      this.isEditMode = false;
    },
  },
};
</script>
