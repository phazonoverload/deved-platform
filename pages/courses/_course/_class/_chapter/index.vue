<template>
  <div class="Course Chapter Vlt-container">
    <div class="Vlt-grid Vlt-grid--stack-flush">
      <div class="Vlt-col"></div>
      <div class="Vlt-col Vlt-col--2of3">
        <div class="Vlt-card" property="mainEntityOfPage">
          <div class="Vlt-card__header">
            <h1 property="headline">{{ chapter.title }}</h1>
          </div>
          <hr class="hr--short Vlt-gradient--blue-to-pink Vlt-margin--A-top2" />
          <div class="Vlt-card__content Vlt-margin--A-top3" property="articleBody">
            <Video :id="chapter.youtube" />
            <nuxt-content :document="chapter" />
          </div>
        </div>
      </div>
      <div class="Vlt-col"></div>
    </div>
    <div class="Vlt-grid Vlt-grid--stack-flush">
      <div class="Vlt-col" />
      <div class="Vlt-col Vlt-col--2of3">
        <prev-next :prev="prev" :next="next" end="End of class 🎉" />
      </div>
      <div class="Vlt-col" />
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const chapter = await $content(`courses/classes/chapters/${params.chapter}`)
        .fetch()
        .catch(err => error({ statusCode: 404, message: 'Page not found', err }))

      const surroundingChapters = await $content('courses/classes/chapters')
        .where({ class: { $eq: params.class } })
        .sortBy('order', 'asc')
        .surround(params.chapter)
        .fetch()

      const [prev, next] = surroundingChapters.map(c => {
        if(c) {
          return {
            ...c,
            slug: `/courses/${params.course}/${c.class}/${c.slug}`
          }
        } else {
          return c
        }
      })

      return { chapter, prev, next }
    } catch (e) {
      console.log(e)
      error(e)
      return false
    }
  }
}
</script>

<style scoped>
.Chapter {
  margin-top: 1em;
}
.Vlt-card__header {
  padding-bottom: 0;
}
.nuxt-content:not(:empty) {
  margin-top: 2em;
}
.nuxt-content >>> pre {
  border-radius: 8px;
  padding: 1em;
  background: #131415;
  color: #c2c4cc;
  margin: 35px -30px;
  font-size: 16px;
  line-height: 1.4;
  padding-left: 27px;
}

.nuxt-content >>> pre code {
  background: #131415;
  color: #c2c4cc;
}
</style>
