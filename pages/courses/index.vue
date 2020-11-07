<template>
  <div>
    <header class="full-width">
      <PageHero>Courses from the Vonage Team</PageHero>
    </header>
    <div class="Vlt-container">
      <Breadcrumbs />
      <nuxt-link
        v-for="course in courses"
        :key="course.slug"
        :to="`/courses/${course.slug}`"
        class="Vlt-card Vlt-bg-white course"
      >
        <img :src="course.thumbnail" :alt="course.title" />
        <div class="info">
          <h1 property="headline">
            {{ course.title }}
          </h1>
          <h2 class="summary">{{ course.summary }}</h2>
        </div>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const courses = await $content('courses').fetch()
      return { courses }
    } catch (e) {
      error(e)
      return false
    }
  },
}
</script>

<style scoped>
.header {
  text-align: center;
  padding: 2em 0;
}
.full-width {
  width: 100vw;
}
.Vlt-container {
  padding: 0 1em;
}
img {
  border-radius: 6px;
  margin-bottom: 1em;
}
h1 {
  margin-bottom: 1rem;
}
h2 {
  margin-top: 0;
  margin-bottom: 0;
}
</style>
