<template>
  <div class="article-meta">
    <nuxt-link :to="{
      name: 'profile',
      params: {
        username: article.author.username
      }
    }">
      <img :src="article.author.image" />
    </nuxt-link>
    <div class="info">
      <nuxt-link class="author" :to="{
        name: 'profile',
        params: {
          username: article.author.username
        }
      }">
        {{ article.author.username }}
      </nuxt-link>
      <span class="date">{{ article.createdAt | date('MMM DD, YYYY') }}</span>
    </div>
    <button
      class="btn btn-sm btn-outline-secondary"
      :class="{
        active: article.author.following
      }"
      @click="onFollow"
    >
      <i class="ion-plus-round"></i>
      &nbsp;
      {{!article.author.following ? 'Follow Gerome' : 'Unfollow Gerome'}} <span class="counter"></span>
    </button>
    &nbsp;&nbsp;
    <button
      class="btn btn-sm btn-outline-primary"
      :class="{
        active: article.favorited
      }"
      @click="onFavorite()"
    >
      <i class="ion-heart"></i>
      &nbsp;
      {{ !article.favorited ? 'Favorite Article' : 'Unfavorite Article' }} <span class="counter">({{ article.favoritesCount }})</span>
    </button>
  </div>
</template>

<script>
export default {
  name: 'ArticleMeta',
  props: {
    article: {
      type: Object,
      required: true
    }
  },
  methods: {
    onFavorite () {
      this.$emit('onFavorite')
    },
    async onFollow () {
      this.$emit('onFollow')
    }
  }
}
</script>

<style>

</style>
