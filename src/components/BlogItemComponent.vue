<template>
  <v-flex xs6>
    <v-card :to="`/blog/${blog.id}`">
      <v-img :src="blog.photo ? apiDomain + blog.photo : 'https://www.ilmubahasainggris.com/wp-content/uploads/2017/03/NGC.jpg'" class="white--text" height="200px">
        <v-card-title class="fill-height align-end" v-text="blog.title"></v-card-title>
      </v-img>

      <v-card-actions>
        <v-progress-linear color="blue-grey" height="7"></v-progress-linear>
      </v-card-actions>

      <!-- <v-card-actions>
        <span>Click for details</span>
      </v-card-actions> -->
      <h3>{{ blog.title.substring(0, 15) }}...</h3>
      <p>{{ blog.description }}</p>
    </v-card>
  </v-flex>
</template>

<script>
export default {
  data: () => ({
    apiDomain: "https://demo-api-vue.sanbercloud.com",
  }),
  props: ["blog"],
};
</script>
