<template>
  <div class="post">
    <h2 class="post__title">{{ post.title }}</h2>
    <h2 class="post__subtitle">{{ post.subtitle }}</h2>
    <img :src="postImage" class="w-full my-3" />
    <p class="post__hour">{{ formatDate(post.published_at) }}</p>
    <div class="post__content" v-html="post.content"></div>
  </div>
</template>

<script>
import moment from "moment";

import { mapGetters } from "vuex";

export default {
  name: "Post",
  data: function () {
    return {};
  },
  mounted() {
    this.$store.dispatch("getPost", this.$route.params.id);
  },
  unmounted() {
    this.$store.dispatch("clearPost");
  },
  computed: {
    ...mapGetters(["getPost"]),
    post() {
      return this.getPost;
    },
    postImage() {
      var url = "/";
      if (this.post.image) {
        console.log("Sim", this.post.image);
        url = this.ROOT_API + this.post.image.url;
      }
      return url;
    },
    ROOT_API() {
      return process.env.VUE_APP_ROOT_API;
    }
  },
  methods: {
    formatDate(data) {
      moment.locale("pt-br");
      return moment.utc(data).fromNow();
    },
    loading(a) {
      if (a.length > 0) {
        return false;
      }
      return true;
    },
  },
};
</script>

<style lang="scss">
.post {
  .post__title {
    @apply text-2xl;
  }
  .post__subtitle {
    @apply text-lg text-gray-700;
  }
  .post__hour {
    @apply text-sm text-gray-600;
  }
  .post__content {
    @apply py-1;
  }
}

.dark-theme {
  .post {
    .post__title {
      @apply text-white;
    }
    .post__content {
      @apply text-white;
    }
  }
}
</style>