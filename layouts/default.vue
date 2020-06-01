<template>
  <div
    :class="
      `site-layout-width-${$siteConfig.layout.width} posts-theme-${$siteConfig.posts.theme}`
    "
  >
    <site-hero
      title="Un éléphant dans la pièce"
      subtitle="Un blogue sur le sens caché des films contemporains"
      image="/elephant-logo.png"
    >
    </site-hero>
    <site-nav />
    <main-section theme="sidebar-right">
      <template v-slot:default>
        <!-- All Posts -->
        <nuxt />
      </template>
      <template v-slot:sidebar>
        <div class="content">
          <h3>
            John the Mercer
          </h3>
          <img src="/aristote.jpg" />
          <p>
            Professeur, politicologue et surtout grand amateur de cinéma, John
            the Mercer entend partager avec vous son amour du cinéma et ses
            analyses herméneutiques.
          </p>
          <h3 class="subtitle">
            All Categories
          </h3>
          <div class="panel">
            <nuxt-link
              v-for="cat in allCats"
              :key="cat.slug"
              :to="`/categories/${cat.slug}`"
              :class="{
                'panel-block': true,
                'is-active': cat.slug === $route.params.single
              }"
            >
              {{ cat.name }}
            </nuxt-link>
          </div>
          <h3>Subscribe</h3>
          <news-letter-form />
        </div>
      </template>
    </main-section>
    <!-- <news-letter-slide-out v-if="$siteConfig.newsletter.on" /> -->
    <site-footer></site-footer>
  </div>
</template>

<script>
import 'animate.css/animate.min.css'
import NewsLetterForm from '~/components/NewsLetterForm'
export default {
  transition: 'slide-fade',
  components: { NewsLetterForm },
  head() {
    return {
      title: `${this.$store.state.title} | ${this.$siteConfig.siteName}`,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.$store.state.subtitle
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.$store.state.subtitle
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: this.$store.state.title
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: this.$store.state.featureImage
            ? (process.env.URL ? process.env.URL : '') +
              require(`~/assets${this.$store.state.featureImage}`)
            : ''
        },
        {
          hid: 'og:url',
          property: 'og:url',
          content: this.url
        },
        {
          hid: 'twitter:card',
          name: 'twitter:card',
          content: `summary_large_image`
        },
        {
          hid: 'og:site_name',
          name: 'og:site_name',
          content: this.$siteConfig.siteName
        }
      ]
    }
  },
  data() {
    return {
      allCats: [],
      items: [
        {
          title: 'Home',
          icon: 'home',
          to: { name: 'index' }
        },
        {
          title: 'Inspire',
          icon: 'lightbulb',
          to: { name: 'inspire' }
        }
      ]
    }
  },
  watch: {
    $route(to, from) {
      this.$eventBus.$emit('route-changed', this.$route)
    }
  },
  mounted() {
    this.$cms.lifeCycleHooks.mounted()
  },
  beforeCreate() {
    this.$cms.lifeCycleHooks.beforeCreate()
  },
  async created() {
    this.$cms.lifeCycleHooks.created()
    this.allCats = await this.$cms.category.getAll()
  },
  beforeMount() {
    this.$cms.lifeCycleHooks.beforeMount()
  },
  beforeUpdate() {
    this.$cms.lifeCycleHooks.beforeUpdate()
  },
  updated() {
    this.$cms.lifeCycleHooks.updated()
  },
  beforeDestroy() {
    this.$cms.lifeCycleHooks.beforeDestroy()
  },
  destroy() {
    this.$cms.lifeCycleHooks.destroy()
  }
}
</script>
