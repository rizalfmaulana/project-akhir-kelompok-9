<template>
  <v-card>
    <v-divider></v-divider>
    <v-container fluid>
      <v-form ref="form" @submit="submitForm($event)">
        <v-text-field v-model="title" label="Title" required append-icon="mdi-format-title"></v-text-field>
        <v-text-field v-model="description" label="Description" required append-icon="mdi-subtitles"></v-text-field>
        <div class="text-xs-center">
          <v-btn type="submit" color="success lighten-1">
            Add Blog
            <v-icon right dark>mdi-plus-box-multiple-outline</v-icon>
          </v-btn>
        </div>
      </v-form>
    </v-container>
  </v-card>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
export default {
  data() {
    return {
      title: "",
      description: "",
    };
  },

  computed: {
    ...mapGetters({
      token: "auth/token",
    }),
  },

  methods: {
    ...mapActions({
      setAlert: "alert/set",
      setToken: "auth/setToken",
    }),

    close() {
      this.$emit("closed", false);
    },

    submitForm: function(event) {
      event.preventDefault();
      console.log(this.token);
      console.log(this.title, this.description);

      let formData = new FormData();
      formData.append("title", this.title);
      formData.append("description", this.description);

      const config = {
        method: "post",
        url: "https://demo-api-vue.sanbercloud.com/api/v2/blog",
        data: formData,
        headers: {
          Authorization: "Bearer" + this.token,
        },
      };

      this.axios(config)
        .then((response) => {
          console.log(response.data);
          this.setAlert({
            status: true,
            color: "success",
            text: "Blog berhasil ditambahkan",
          });
          this.close();
          this.$router.push({ path: "/blogs" });
        })
        .catch((response) => {
          console.log(response);
          this.setAlert({
            status: true,
            color: "error",
            text: "Silahkan login dahulu",
          });
        });
    },
  },
};
</script>
