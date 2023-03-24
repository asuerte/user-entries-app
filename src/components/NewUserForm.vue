<template>
  <v-container>
    <v-form v-model="valid" @submit.prevent="submit" ref="form">
      <v-row>
        <v-col cols="12">
          <v-text-field
            v-model="firstName"
            :rules="nameRules"
            :counter="10"
            label="First name"
            required
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12">
          <v-text-field
            v-model="lastName"
            :rules="nameRules"
            :counter="10"
            label="Last name"
            required
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <v-text-field
            v-model="email"
            :rules="emailRules"
            label="E-mail"
            required
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <v-btn type="submit" width="100%" color="black">{{
            buttonLabel
          }}</v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
export default {
  name: "NewUserForm",
  props: {
    selectedUser: { type: Object, default: () => ({}) },
    isEditMode: { type: Boolean, default: false },
  },
  data() {
    return {
      valid: false,
      firstName: "",
      lastName: "",
      nameRules: [
        (value) => {
          if (value) return true;

          return "Name is requred.";
        },
        (value) => {
          if (value?.length <= 10) return true;

          return "Name must be less than 10 characters.";
        },
      ],
      email: "",
      emailRules: [
        (value) => {
          if (value) return true;

          return "E-mail is requred.";
        },
        (value) => {
          if (/.+@.+\..+/.test(value)) return true;

          return "E-mail must be valid.";
        },
      ],
    };
  },
  watch: {
    selectedUser() {
      if (this.isEditMode) {
        this.firstName = this.selectedUser.firstName;
        this.lastName = this.selectedUser.lastName;
        this.email = this.selectedUser.email;
      }
    },
  },
  computed: {
    buttonLabel() {
      return this.isEditMode ? "Save" : "Add";
    },
  },
  methods: {
    submit() {
      if (this.valid) {
        if (this.isEditMode) {
          this.emitUpdatedEntry();
        } else {
          this.emitNewEntry();
        }

        // Add timer to reset as reset functionality gets executed first
        setTimeout(() => {
          this.resetFormInput();
        }, 1)
      }
    },
    emitUpdatedEntry() {
      let userInfo = {
        id: this.selectedUser.id,
        fullName: `${this.firstName} ${this.lastName}`,
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email,
      };
      this.$emit("updateExistingUser", userInfo);
    },
    emitNewEntry() {
      let userInfo = {
        id: `${Math.round(Math.random() * 1000)}`,
        fullName: `${this.firstName} ${this.lastName}`,
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email,
      };
      this.$emit("addNewUser", userInfo);
    },
    resetFormInput() {
      this.$refs.form.reset()
    },
  },
};
</script>
