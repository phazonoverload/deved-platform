<template>
<div class="Course Class Vlt-container">
    <div class="Vlt-grid Vlt-grid--stack-flush">
      <div class="Vlt-col" />
      <div class="Vlt-col Vlt-col--2of3">
        <div
          class="Vlt-card"
          property="mainEntityOfPage"
        >
          <div class="Vlt-card__header">
            <h1 property="headline">{{ singleClass.title }}</h1>
          </div>
          <hr class="hr--short Vlt-gradient--blue-to-pink Vlt-margin--A-top2" />
          <div
            class="Vlt-card__content Vlt-margin--A-top3"
            property="articleBody"
          >
            <p class="summary"><b>{{singleClass.description}}</b></p>
            <Video :id="singleClass.youtube" />
            <small>{{chapters.length}} Chapters &bull; {{runTime}} Runtime</small>
            <Listing :links="chapters" />
          </div>
        </div>
      </div>
      <div class="Vlt-col" />
    </div>
    <div class="Vlt-grid Vlt-grid--stack-flush">
      <div class="Vlt-col" />
      <div class="Vlt-col Vlt-col--2of3">
        <!-- Inner - prev/next goes here -->
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

      const singleClass = await $content(`courses/classes/${params.class}`)
        .fetch()
        .catch(err => error({ statusCode: 404, message: 'Page not found', err }))

      const chapters = await $content(`courses/classes/chapters`)
        .where({ class: { $eq: params.class } })
        .sortBy('order', 'asc')
        .fetch()

      const runTime = courseLength(chapters.map(c => c.length))

      return {
        course,
        singleClass,
        runTime,
        chapters: chapters.map(c => {
          return {
            ...c,
            url: `/courses/${course.slug}/${singleClass.slug}/${c.slug}`,
            text: {
              main: c.title,
              secondary: `${Math.ceil(c.length/60)} mins`
            }
          }
        })
      }
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
.Class {
  margin-top: 1em;
}
.Vlt-card__header {
  padding-bottom: 0;
}
.summary {
  font-size: 1.25em;
  margin-bottom: 1.25em;
}
.video {
  margin-bottom: 1.5rem;
}
small {
  margin-bottom: 0.5rem;
  display: block;
}
</style>
