<template>
  <div class="Module">
    <div class="icon">
      <img :src="module.icon" :alt="module.title" />
    </div>
    <div class="content">
      <h3>{{ module.title }}</h3>
      <p class="desc">{{ module.description }}</p>
      <ul>
        <li
          v-for="(lo, i) in module.key_learnings"
          :key="i"
          class="Vlt-list-item"
        >
          <span class="Vlt-list-item__icon">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              class="stroke-current stroke-1 icon-size"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </span>
          <span class="Vlt-list-item__text">{{ lo }}</span>
        </li>
      </ul>
      <NuxtLink
        class="Vlt-btn Vlt-btn--primary"
        :to="`/courses/${module.course}/${module.slug}`"
        >Go to class</NuxtLink
      >
    </div>
  </div>
</template>

<script>
export default {
  props: {
    module: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style scoped>
.Module {
  display: grid;
  grid-template-columns: 100px auto;
  gap: 3em;
  margin-top: 2em;
}
img {
  margin-top: 0.25em;
}
.desc {
  font-size: 1.25em;
  margin-bottom: 1em;
}
ul {
  margin-bottom: 1em;
}
@media only screen and (max-width: 515px) {
  .Module {
    grid-template-columns: 1fr;
    gap: 1em;
  }
  img {
    width: 100px;
  }
}
</style>
