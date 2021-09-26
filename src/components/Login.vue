<template>
  <v-card>
    <v-toolbar dark color="success">
      <v-btn icon dark @click.native="close">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title>Login</v-toolbar-title>
    </v-toolbar>
    <v-divider></v-divider>
    <v-container fluid>
      <v-form ref="form">
        <v-text-field v-model="email" label="E-mail" required append-icon="mdi-email"></v-text-field>
        <v-text-field v-model="password" label="password" counter :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'" :type="showPassword ? 'text' : 'password'" @click:append="showPassword = !showPassword"></v-text-field>
        <div class="text-xs-center">
          <v-btn color="success lighten-1" @click="submit">
            Login
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
      apiDomain: "https://demo-api-vue.sanbercloud.com",
    };
  },
  methods: {
    ...mapActions({
      setAlert: "alert/set",
      setToken: "auth/setToken",
    }),
    close() {
      this.$emit("closed", false);
    },
    submit() {
      let formData = new FormData();
      formData.append("email", this.email);
      formData.append("password", this.password);
      const config = {
        method: "post",
        url: this.apiDomain + "/api/v2/auth/login",
        data: formData,
      };

      this.axios(config)
        .then((response) => {
          console.log(response.data);
          this.setToken(response.data.access_token);

          this.setAlert({
            status: true,
            color: "success",
            text: "login berhasil",
          });
          this.close();
        })
        .catch((response) => {
          console.log(response);
          this.setAlert({
            status: true,
            color: "error",
            text: "login gagal",
          });
        });
    },
  },
};
</script>
