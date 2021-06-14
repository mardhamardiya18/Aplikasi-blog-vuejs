<template>
  <v-container class="ma-0 pa-0" grid-list-sm>
    <div class="text-right">
      <v-btn small text to="/blogs" class="blue--text">
        All Blogs <v-icon>mdi-chevrom-right</v-icon>
      </v-btn>
    </div>
    <v-layout wrap>
      <blog-item-component
        v-for="blog in blogs"
        :key="`blog-` + blog.id"
        :blog="blog"
      >
      </blog-item-component>
    </v-layout>

    <button @click="increment(10)">Tambah</button>
    {{ tambah }}
  </v-container>
</template>

<script>
import BlogItemComponent from "../components/BlogItemComponent.vue";
import { mapGetters, mapMutations } from "vuex";

export default {
  data: () => ({
    apiDomain: "http://demo-api-vue.sanbercloud.com",
    blogs: [],
  }),
  components: {
    "blog-item-component": BlogItemComponent,
  },

  computed: {
    ...mapGetters({
      tambah: "counter/count",
    }),
  },

  created() {
    const config = {
      method: "get",
      url: this.apiDomain + "/api/v2/blog/random/3",
    };

    this.axios(config)
      .then((Response) => {
        let { blogs } = Response.data;
        this.blogs = blogs;

        console.log(this.blogs);
      })
      .catch((error) => {
        console.log(error);
      });
  },

  methods: {
    ...mapMutations({
      increment: "counter/increment",
    }),
  },
};
</script>
