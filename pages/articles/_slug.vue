<template>
  <div class="flex flex-col m-auto w-1/2 py-10">
    <h1 class="text-green-500 m-auto text-4xl">{{ article.title }}</h1>
    <p class="text-gray-400 m-auto text-md mb-10">
      Published on {{ article.date }}
    </p>
    <nuxt-content :document="article" />
    <hr class="m-10" />
    <p class="text-xl mb-5">You may want to read</p>
    <div class="flex items-stretch">
      <nuxt-link
        v-if="prev"
        :to="{ name: 'articles-slug', params: { slug: prev.slug } }"
        class="border shadow-md p-8 text-2xl"
        >&lt; {{ prev.title }}</nuxt-link
      >&nbsp;
      <nuxt-link
        v-if="next"
        :to="{ name: 'articles-slug', params: { slug: next.slug } }"
        class="border shadow-md p-8 text-2xl"
        >{{ next.title }} &gt;</nuxt-link
      >
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    let article;

    try {
      article = await $content("articles", params.slug).fetch();
    } catch (error) {
      error({ message: "Article not found" });
    }

    const [prev, next] = await $content("articles")
      .only(["title", "slug"])
      .sortBy("date", "desc")
      .surround(params.slug)
      .fetch();

    return { article, prev, next };
  }
};
</script>

<style scoped>
.nuxt-content p {
  line-height: 1.75;
  letter-spacing: 0.75px;
}

.nuxt-content h2 {
  margin-bottom: 1rem;
  margin-top: 2rem;
  font-size: 1.75rem;
}
</style>
