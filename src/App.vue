<template>
  <div id="app">
    <div class="row">
      <div class="col s4 m4" v-for="(post, index) in posts" :key="index">
        <div class="card">
          <div class="card-image">
            <img
              v-if="post._embedded['wp:featuredmedia']"
              :src="post._embedded['wp:featuredmedia'][0].source_url"
            />
            <span class="card-title">{{ post.title.rendered }}</span>
          </div>
          <div class="card-content" v-html="post.excerpt.rendered"></div>
          <div class="card-action">
            <a href="#">{{ post.title.rendered }}</a>
            <p>
              <strong>Published: {{ getPostDate(post.date) }}</strong>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  data() {
    return {
      // Wordpress Posts Endpoint
      postsUrl: "https://www.citimuzik.com/wp-json/wp/v2/posts",
      queryOptions: {
        per_page: 10, // Only retrieve the 10 most recent blog posts.
        page: 4, // Current page of the collection.
        _embed: true //Response should include embedded resources.
      },
      // Returned Posts in an Array
      posts: []
    };
  },
  methods: {
    // Get Recent Posts From WordPress Site
    getRecentMessages() {
      axios
        .get(this.postsUrl, { params: this.queryOptions })
        .then(response => {
          this.posts = response.data;
          console.log("Posts retrieved!");
          console.log(this.posts);
        })
        .catch(error => {
          console.log(error);
        });
    },
    getPostDate(date) {
      return moment(date).format("lll");
    }
  },
  mounted() {
    this.getRecentMessages();
  }
};
</script>

<style>
#app {
  padding: 100px;
}
</style>