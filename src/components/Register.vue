<template>
  <v-card>
    <v-toolbar dark color="red darken-1">
      <v-btn icon dark @click.native="close">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title>Register</v-toolbar-title>
    </v-toolbar>
    <v-divider></v-divider>
    <v-container fluid>
      <v-form ref="form">
        <v-text-field
          v-model="email"
          label="Email"
          required
          append-icon="mdi-email"
        ></v-text-field>
        <v-text-field
          v-model="password"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          :type="showPassword ? 'text' : 'password'"
          label="Password"
          counter
          @click:append="showPassword = !showPassword"
        ></v-text-field>
        <v-text-field
          v-model="name"
          label="Nama Lengkap"
          required
          append-icon="mdi-account"
        ></v-text-field>

        <div class="text-xs-center">
          <v-btn color="success lighten-1" @click="submit">
            Register
            <v-icon right dark>mdi-lock-open</v-icon>
          </v-btn>
        </div>
      </v-form>
    </v-container>
  </v-card>
</template>
<script>
import { mapActions } from "vuex";

export default {
  data() {
    return {
      email: "",
      showPassword: false,
      password: "",
      name: "",
      photo: null,
      apiDomain: "http://demo-api-vue.sanbercloud.com",
    };
  },
  methods: {
    ...mapActions({
      setAlert: "alert/set",
    }),

    close() {
      this.$emit("closed", false);
    },
    submit() {
      const config = {
        method: "post",
        url: "http://demo-api-vue.sanbercloud.com/api/v2/auth/register",
        data: {
          name: this.name,
          email: this.email,
          password: this.password,
          photo_profile: this.photo,
        },
      };
      this.axios(config)
        .then((Response) => {
          console.log(Response.data);
          this.setAlert({
            status: true,
            color: "success",
            text: "Registrasi berhasil",
          });
          this.close();
        })
        .catch((Response) => {
          console.log(Response);
          this.setAlert({
            status: true,
            color: "error",
            text: "Registrasi Gagal",
          });
        });
    },
  },
};
</script>