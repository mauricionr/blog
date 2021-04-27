<template>
  <div class="container">
    <div v-if="post">
      <h1 class="title" v-text="post.title" />
      <div>
        <SanityImage :asset-id="post.mainImage.asset._ref" auto="format" />
      </div>
      <div class="content">
        <SanityContent v-for="body in post.body" :blocks="[body]" :key="body._id" />
      </div>
    </div>
    <h4><a href="/">← Go back</a></h4>
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
  async asyncData({ params, $sanity }) {
    console.log(params)
    const query = groq`*[_type == "post" && slug.current == "${params.slug}"]`
    const post = await $sanity.fetch(query)
    const response = { post: post[0] }
    console.log(response)
    return response
  }
}
</script>

<style>
.container {
  margin: 2rem;
  min-height: 100vh;
}

.content {
  margin: 2rem 0;
  max-width: 38rem;
}

p { margin: 1rem 0; }
</style>