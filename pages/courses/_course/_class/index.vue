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
            <small>{{chapters.length}} Chapters &bull; {{runTime}} Minutes Total</small>
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

      let chapters = await $content(`courses/classes/chapters`)
        .where({ class: { $eq: params.class } })
        .sortBy('order', 'asc')
        .fetch()

      chapters = chapters.map(c => {
        return {
          ...c,
          url: `/courses/${course.slug}/${singleClass.slug}/${c.slug}`,
          mins: Math.round(c.length/60),
          text: {
            main: c.title,
            secondary: `${Math.round(c.length/60)} mins`
          }
        }
      })

      const runTime = chapters.map(c => c.mins).reduce((a, v) => a + v, 0)

      return {
        course,
        singleClass,
        runTime,
        chapters
      }
    } catch (e) {
      error(e)
      return false
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
