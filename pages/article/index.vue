<!--  -->
<template>
  <div class="article-page">
    <div class="banner">
      <div class="container">
        <h1>{{ article.title }}</h1>
        <article-meta @onFollow="onFollow" @onFavorite="onFavorite" :article="article" />
      </div>
    </div>

    <div class="container page">
      <div class="row article-content">
        <div class="col-md-12" v-html="article.body"></div>
      </div>

      <hr />

      <div class="article-actions">
        <article-meta @onFollow="onFollow" @onFavorite="onFavorite" :article="article" />
      </div>

      <div class="row">
        <div class="col-xs-12 col-md-8 offset-md-2">
          <article-comments :article="article" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getArticle, addFavorite, deleteFavorite, follow, followCancel } from '@/api/article'
import MarkdownIt from 'markdown-it'
import ArticleMeta from './components/article-meta'
import ArticleComments from './components/article-comments'
export default {
  name: "ArticleIndex",
  async asyncData ({ params }) {
    const { data } = await getArticle(params.slug)
    const { article } = data
    const md = new MarkdownIt()
    article.body = md.render(article.body)
    return {
      article
    }
  },
  data() {
    return {};
  },
  components: {
    ArticleMeta,
    ArticleComments
  },
  head () {
    return {
      title: `${this.article.title} - RealWorld`,
      meta: [
        { hid: 'description', name: 'description', content: this.article.description }
      ]
    }
  },
  mounted() {},
  methods: {
    async onFollow () {
      const followOrCancel = !this.article.author.following ? follow : followCancel
      const { data } = await followOrCancel(this.article.author.username);
      this.article.author.following = data.profile.following;
    },
    async onFavorite () {
      this.article.favoriteDisabled = true
      if (this.article.favorited) {
        // 取消点赞
        await deleteFavorite(this.article.slug)
        this.article.favorited = false
        this.article.favoritesCount += -1
      } else {
        // 添加点赞
        await addFavorite(this.article.slug)
        this.article.favorited = true
        this.article.favoritesCount += 1
      }
      this.article.favoriteDisabled = false
    }
  },
};
</script>

<style lang="scss" scoped>
</style>