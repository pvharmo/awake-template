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
            Categories
          </h3>
          <ul>
            <li v-for="cat in allCats" :key="cat.slug">
              <nuxt-link :to="`/categories/${cat.slug}`">
                {{ cat.name }}
              </nuxt-link>
            </li>
          </ul>
          <h3 class="subtitle">Subscribe</h3>
          <news-letter-form class="newsletter-form" />
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

<style scoped>
.content {
  margin-left: 10px;
}
.content h3 {
  font-size: 18px;
}
.content p {
  font-weight: 700;
  font-size: 15px;
  line-height: 31px;
}
.content .subtitle {
  padding-top: 30px;
}

ul {
  margin-left: 0;
}

li {
  list-style: none;
  padding: 6px 0 0;
  margin: 6px 0 0;
  border-top: 1px solid #eee;
}

li a {
  line-height: 31px;
  font-size: 15px;
  color: #1c7c7c;
  transition: all 0.2s ease-in-out;
}
li a:hover {
  color: #666;
}

.newsletter-form {
  padding-top: 10px;
}

.content img {
  width: 85%;
  margin-left: 10px;
}
</style>
