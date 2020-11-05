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
          </div>
        </div>
      </div>
      <div class="Vlt-col"></div>
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

      return { chapter }
    } catch (e) {
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
</style>
