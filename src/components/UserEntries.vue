<template>
  <v-container>
    <v-card class="mx-auto">
      <v-list lines="three" bg-color="yellow-lighten-4">
        <v-list-subheader class="text-h6 text-black">ENTRIES</v-list-subheader>
        <v-list-item
          v-for="entry in userList"
          :key="entry.id"
          :title="entry.fullName"
          :subtitle="entry.email"
        >
          <template v-slot:title="{ title }">
            <span class="text-black">{{ title }}</span>
          </template>
          <template v-slot:subtitle="{ subtitle }">
            <span class="text-black">{{ subtitle }}</span>
          </template>
          <template v-slot:append>
            <v-btn class="mx-2 bg-white" @click="updateEntry(entry)"
              >Update</v-btn
            >
            <v-btn class="mx-2 bg-white" @click="removeEntry(entry)"
              >Remove</v-btn
            >
          </template>
        </v-list-item>
      </v-list>
    </v-card>
  </v-container>
</template>

<script>
import NewUserForm from "@/components/NewUserForm.vue";

export default {
  name: "UserEntries",
  components: { NewUserForm },
  props: { userList: { type: Array } },
  data() {
    return {
      selectedUser: undefined,
    };
  },
  methods: {
    updateEntry(user) {
      this.$emit("emitUpdateUser", user);
    },
    removeEntry(entry) {
      this.$emit("emitRemoveUser", entry.id);
    },
  },
};
</script>
