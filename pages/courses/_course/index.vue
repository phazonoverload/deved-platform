<template>
  <div class="Course Course__landing Vlt-container">
    <div class="Vlt-grid Vlt-grid--stack-flush">
      <div class="Vlt-col" />
      <div class="Vlt-col Vlt-col--2of3">
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
            <h2>{{course.summary}}</h2>
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
      return { course, classes, chapters, runTime }
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
  }
}
</script>

<style scoped>
.Course__landing {
  margin-top: 1em;
}
.Vlt-card__header img {
  border-radius: 6px;
}
</style>

<style>
.Author-col {
  padding: 0;
}
</style>
