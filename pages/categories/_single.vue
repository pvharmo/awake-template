<template>
  <div id="category-page" class="page-wrapper category-page">
    <!-- Posts in Category -->
    <posts-grid :category="[$store.state.name]" :per-row="2" />
  </div>
</template>
<script>
import { setPageData } from '../../helper'
export default {
  data() {
    return {
      allCats: []
    }
  },
  fetch({ store, params }) {
    setPageData(store, { resource: 'category', slug: params.single })
  },
  async created() {
    this.allCats = await this.$cms.category.getAll()
  }
}
</script>
