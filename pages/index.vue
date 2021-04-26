<template>
  <div class="container">
    <div>
      <h1 class="title">
        Built with tests
      </h1>
    </div>
    <div class="posts">
      <div v-for="post in posts" :key="post._id">
        <h2><a v-bind:href="post.slug.current" v-text="post.title" /></h2>
        <div>
          <SanityImage :asset-id="post.mainImage.asset._ref" auto="format" />
        </div>
        <div class="summary" v-if="post">
          <SanityContent :blocks="[post.body[0]]" />
        </div>
        <hr />
      </div>
    </div>
  </div>
</template>

<script>
import { groq } from '@nuxtjs/sanity'
import { SanityContent } from '@nuxtjs/sanity/dist/components/sanity-content'
import { SanityImage } from '@nuxtjs/sanity/dist/components/sanity-image';

export default {
  components: {
    SanityContent,
    SanityImage
  },
  async asyncData({ $sanity }) {
    const query = groq`*[_type == "post"]`
    const posts = await $sanity.fetch(query)
    return { posts }
  },
}
</script>

<style>
.container {
  margin: 2rem;
  min-height: 100vh;
}
.posts {
  margin: 2rem 0;
}
.summary { margin-top: 0.5rem; }
img {
  max-width: 300px;
}
</style>
