<template>
  <v-card>
    <v-toolbar dark color="success">
      <v-btn icon dark @click.native="close">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title>Register</v-toolbar-title>
    </v-toolbar>
    <v-divider></v-divider>
    <v-container fluid>
      <v-form ref="form">
        <v-text-field v-model="email" label="E-mail" required append-icon="mdi-email"></v-text-field>
        <v-text-field v-model="name" label="Name" required append-icon="mdi-account"></v-text-field>
        <v-text-field v-model="password" label="Password" counter :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'" :type="showPassword ? 'text' : 'password'" @click:append="showPassword = !showPassword"></v-text-field>
        <input type="file" name="photo" ref="photo" />
        <div class="text-xs-center">
          <v-btn color="success lighten-1" @click="submit">
            Register
            <v-icon right dark>mdi-account-multiple-plus</v-icon>
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
      apiDomain: "https://demo-api-vue.sanbercloud.com",
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
      let file = this.$refs.photo.files[0];
      let formData = new FormData();
      formData.append("email", this.email);
      formData.append("password", this.password);
      formData.append("name", this.name);
      formData.append("photo_profile", file);
      const config = {
        method: "post",
        url: this.apiDomain + "/api/v2/auth/register",
        data: formData,
      };

      this.axios(config)
        .then((response) => {
          console.log(response.data);

          this.setAlert({
            status: true,
            color: "success",
            text: "register berhasil",
          });
          this.close();
        })
        .catch((response) => {
          console.log(response);
          this.setAlert({
            status: true,
            color: "error",
            text: "register gagal",
          });
        });
    },
  },
};
</script>
