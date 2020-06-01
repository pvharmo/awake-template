<template>
  <div id="post-page" class="page-wrapper post-page">
    <div class="post-wrapper">
      <markdown :markdown="$store.state.content" />
      <div class="other-posts">
        <h6 class="subtitle is-size-4">
          Related Posts
        </h6>
        <!-- Related Posts -->
        <posts-grid :number="3" :category="category" :exclude="slug" />
      </div>
      <disqus-comments :identifier="$route.params.singlePost" />
    </div>
  </div>
</template>
<script>
import { mapState } from 'vuex'
import { setPageData, getFormattedDate } from '../helper'
// import 'highlight.js/styles/github.css'
import Markdown from '~/components/Markdown'
export default {
  components: {
    Markdown
  },
  computed: {
    ...mapState([
      'title',
      'subtitle',
      'featureImage',
      'underSubtitle',
      'author',
      'category',
      'slug'
    ]),
    date() {
      return getFormattedDate(this.$store.state.date)
    },
    url() {
      return `${process.env.URL}/${this.$route.fullPath}`
    }
  },
  fetch({ store, params }) {
    setPageData(store, { resource: 'post', slug: params.singlePost })
  }
}
</script>
<style scoped lang="scss">
.edit-post {
  margin-bottom: 20px;
}
</style>
