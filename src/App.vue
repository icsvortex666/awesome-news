<template>
  <v-app dark>
    <v-navigation-drawer
      fixed
      right
      :clipped="clipped"
      v-model="drawer"
      app
    >
      <v-list two-line dense>
        <article-list
          v-for="(article, idx) in articles"
          v-bind:key="idx"
          v-bind:title="article.title"
          v-bind:description="article.description"
          v-bind:author="article.author"
          v-bind:date="article.publishedAt"
          v-bind:img="article.urlToImage"
          v-bind:url="article.url"
          @clicked-article="displayData"
        />
      </v-list>
    </v-navigation-drawer>

    <v-toolbar
      app
      :clipped-left="clipped"
      flat
    >
      <v-toolbar-title>
        <span v-html="title" />
        <v-icon color="purple">whatshot</v-icon>
      </v-toolbar-title>

      <v-spacer />

      <v-btn icon @click.stop="drawer = !drawer">
        <v-icon>menu</v-icon>
      </v-btn>
    </v-toolbar>

    <v-content>
      <v-container
        fluid
        fill-height
      >
        <v-layout column>
          <article-view  v-bind:article="article" />
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import ArticleList from '@/components/ArticleList.vue';
import ArticleView from '@/components/ArticleView.vue';

export default {
  name: 'App',
  components: {
    ArticleList,
    ArticleView,
  },
  data: () => ({
    clipped: false,
    drawer: true,
    fixed: false,
    right: true,
    title: 'חדשות מדהים',
    articles: [],
    api: {
      url: 'https://newsapi.org/v2/everything?sources=ynet',
      key: 'e3ef295dbf2745cab9ddac02f648444d',
    },
    article: {},
  }),
  created() {
    this.fetchArticles();
  },
  methods: {
    fetchArticles() {
      fetch(`${this.api.url}&apiKey=${this.api.key}`)
        .then(res => res.json())
        .then((data) => {
          this.articles = data.articles;
        })
        .catch((err) => {
          if (err) throw err;
        });
    },
    displayData(article) {
      this.article = article;
    },
  },
};
</script>

<style lang="scss" scoped>
  .no-padding {
    padding: 0;
  }
</style>
