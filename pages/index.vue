<template>
  <div class="container">
    <input
      id="search"
      v-model="q"
      placeholder="Search..."
      class="border rounded-lg shadow-lg p-4 w-auto mb-5"
    />
    <nuxt-link
      v-for="article in articles"
      :key="article.slug"
      class="border shadow-lg p-4 w-auto mb-2 cursor-pointer"
      :to="{ name: 'articles-slug', params: { slug: article.slug } }"
    >
      <p>{{ article.title }}</p>
      <p>{{ article.path }}</p>
    </nuxt-link>
  </div>
</template>

<script>
export default {
  watchQuery: true,
  async asyncData({ $content, route }) {
    const q = route.query.q;
    let query = $content("articles").sortBy("date", "desc");
    if (q) {
      query = query.search(q);
    }
    const articles = await query.fetch();
    return {
      q,
      articles
    };
  },
  watch: {
    q() {
      this.$router
        .replace({ query: this.q ? { q: this.q } : undefined })
        .catch(() => {});
    }
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  padding: 1rem;
}
</style>
