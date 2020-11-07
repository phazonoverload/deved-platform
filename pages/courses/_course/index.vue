<template>
  <div class="Course Course__landing Vlt-container">
    <div class="Vlt-grid Vlt-grid--stack-flush">
      <div class="Vlt-col" />
      <div class="Vlt-col Vlt-col--2of3">
        <Breadcrumbs />
        <div
          class="Vlt-card"
          property="mainEntityOfPage"
        >
          <div v-if="course.thumbnail" class="Vlt-card__header">
            <img
              property="image"
              :src="course.thumbnail"
              :alt="course.title"
              width="100%"
            />
          </div>
          <div class="Vlt-card__header Vlt-margin--A-top3">
            <h1 property="headline">
              {{ course.title }}
            </h1>
            <h2 class='summary'>{{course.summary}}</h2>
            <small>{{classes.length}} Classes &bull; {{chapters.length}} Chapters &bull; {{runTime}} Total</small>
          </div>
          <hr class="hr--short Vlt-gradient--blue-to-pink Vlt-margin--A-top3" />
          <div
            class="Vlt-card__content Vlt-margin--A-top3"
            property="articleBody"
          >
            <nuxt-content :document="course" />
          </div>
          <div class="listing Vlt-margin--A-top3">
            <h2>Through {{classes.length}} classes you will cover...</h2>
            <Module v-for="module in classes" :key="module.path" :module="module" />
          </div>
        </div>
      </div>
      <div class="Vlt-col" />
    </div>
    <div class="Vlt-grid Vlt-grid--stack-flush">
      <div class="Vlt-col" />
      <div class="Vlt-col Vlt-col--2of3">
        <Author :author="course.instructor" type="card" />
      </div>
      <div class="Vlt-col" />
    </div>
  </div>
</template>

<script>
import config from '~/modules/config'
export default {
  async asyncData({ $content, params, error }) {
    try {
      const course = await $content(`courses/${params.course}`)
        .fetch()
        .catch(err => error({ statusCode: 404, message: 'Page not found', err }))

      const classes = await $content(`courses/classes`)
        .where({ course: { $eq: params.course } })
        .sortBy('order', 'asc')
        .fetch()

      const chapters = await $content(`courses/classes/chapters`)
        .where({ class: { $containsAny: classes.map(c => c.slug) } })
        .sortBy('order', 'asc')
        .fetch()

      const runTime = courseLength(chapters.map(c => c.length))
      return { course, classes, chapters, runTime, baseUrl: config.baseUrl, }
    } catch (e) {
      error(e)
      return false
    }

    function courseLength(times) {
      const timestamp = times.reduce((acc, item) => acc + item, 0)
      const h = Math.floor(timestamp / 60 / 60)
      const m = Math.floor(timestamp / 60) - (h * 60)
      return h.toString().padStart(2, '0') + ':' + m.toString().padStart(2, '0')
    }
  },
  head() {
    return {
      title: `${this.course.title}`,
      meta: [
        { hid: 'keywords', name: 'keywords', content:`${this.course.keywords.join(',')}` },
        { hid: 'description', name: 'description', content:`${this.course.summary}` },
        ...this.postMeta()
      ]
    }
  },
  methods: {
    postMeta() {
      let thumbnail = this.course.thumbnail
      if (typeof thumbnail !== 'undefined' && !thumbnail.startsWith('http')) {
        thumbnail = `${this.baseUrl}${thumbnail}`
      }
      return [
        { hid: 'twitter:url', name: 'twitter:url', content: `${this.baseUrl}${this.course.path}` },
        { hid: 'twitter:title', name: 'twitter:title', content: `${this.course.title}${config.baseSplitter}${config.baseTitle}` },
        { hid: 'twitter:description', name: 'twitter:description', content: this.course.summary },
        { hid: 'twitter:image', name: 'twitter:image', content: `${thumbnail || '/images/generic-social-card.png'}` },
        { hid: 'twitter:image:alt', name: 'twitter:image:alt', content: `${this.course.title}${config.baseSplitter}${config.baseBrand}` },
        { hid: 'og:url', property: 'og:url', content: `${this.baseUrl}${this.course.path}` },
        { hid: 'og:title', property: 'og:title', content: `${this.course.title}${config.baseSplitter}${config.baseTitle}` },
        { hid: 'og:description', property: 'og:description', content: this.course.summary },
        { hid: 'og:image', property: 'og:image', content: `${thumbnail || '/images/generic-social-card.png'}` }
      ]
    }
  }
}
</script>

<style scoped>
.Vlt-card__header img {
  border-radius: 6px;
}
.summary {
  margin: 1rem 0 !important;
}
</style>

<style>
.Author-col {
  padding: 0;
}
</style>
